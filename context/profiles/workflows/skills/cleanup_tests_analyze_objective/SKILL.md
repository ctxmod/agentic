---
description: "Judge a single ≤30-file test group against four criteria (redundancy, implementation-pinning, flakiness, cost-vs-value) and emit per-file keep/delete/refactor findings."
binds_to_depth: objective
---

# Cleanup Tests — Analyze Objective

Receive a single test-file group (≤30 files) from the discovery
report, read each test, and judge it against four criteria. Emit
structured findings the acting phase can apply with minimal
re-reading.

## Core Behavior

1. **Consume**: Read the group's file list from the input discovery
   report (passed as `INPUT_REPORT`).
2. **Bound**: Confirm the group has ≤30 files. If the discovery
   report hands over more, halt and report the bound violation —
   the partition step failed and the phase must re-partition.
3. **Read**: Open every test file in the group plus the production
   code it exercises. Cross-reference is required: judging
   implementation-pinning without reading the code under test
   produces noisy findings.
4. **Judge**: Apply the four criteria below to each test.
5. **Report**: Write structured findings to the reserved report path.

## The ≤30-File Bound is Load-Bearing

intent §5 sets the per-group bound at ≤30 files. This is a hard
limit, not a guideline. A single agent analyzing more produces
shallow judgments because context budget is split across too many
files. If `INPUT_REPORT` describes a group larger than 30, halt
with a bound violation rather than processing partially.

## Four Judging Criteria

Each test receives one finding per criterion that applies. A test
may be clean on all four (finding: `keep`). The four criteria
mirror the intent §5 scope:

### 1. Redundancy

The test duplicates assertions another test already covers.
Signals:

- Two tests in the same package assert the same property with
  different fixture setup.
- A test is a strict subset of another test's assertion set.
- A test suite was copied across packages and never diverged.

Finding shape: `delete` (the duplicate) with a pointer to the
canonical test that remains.

### 2. Implementation-pinning

The test asserts on internal details that would break under a
legitimate refactor even though behavior is unchanged. Signals:

- Asserts on private field values or internal state rather than
  observable output.
- Asserts on the exact sequence of internal calls (mock call
  order, function-call count) when only the result matters.
- Hard-codes structure-specific values (e.g. exact SQL string,
  exact HTTP route path) where a contract-level check would do.

Finding shape: `refactor` (rewrite to assert contract, not
implementation) with the specific pinned detail cited by file:line.

**The worst outcome to flag explicitly:** a backward-compatibility
shim exists in production code solely to keep this test passing.
Cite the shim's location (`prod-file:line`) so the phase can
schedule code-debt cleanup alongside test cleanup.

### 3. Flakiness

The test depends on timing, ordering, environment, or network in
ways that produce intermittent failures. Signals:

- Uses `time.Sleep` or hard-coded timeouts where a channel or
  polling loop would do.
- Depends on test ordering (shared global state, `t.Setenv` leaks,
  package-level vars mutated across tests).
- Reaches out to the network or filesystem outside a temp dir.
- Skipped under `-race` or `-short` without a documented reason.

Finding shape: `refactor` (fix the flake) or `delete` (when the
flake has no contract value and the underlying behavior is covered
elsewhere).

### 4. Cost-vs-value

The test is slow and low-value. Signals:

- Runs >1s in isolation without exercising IO-bound behavior that
  justifies the cost.
- Spin up heavy fixtures (full HTTP server, real DB) for a
  property a unit test could check.
- Tests trivial getters/setters or constants.

Finding shape: `delete` (coverage gap is acceptable) or `refactor`
(consolidate into a faster test).

## Report Structure

The analysis report carries one section per file in the group:

```
## <repo-relative path>

### Findings

- criterion: redundancy
  action: delete
  justification: duplicates `TestFoo` in `pkg/foo_test.go`
  canonical: pkg/foo_test.go:TestFoo

- criterion: implementation-pinning
  action: refactor
  justification: asserts private field `server.connCount`
  pinned-detail: pkg/server/server.go:42
  prod-shim: pkg/server/legacy.go:15 (compat shim exists for this test)
```

Each finding carries:

- **criterion** — one of `redundancy`, `implementation-pinning`,
  `flakiness`, `cost-vs-value`.
- **action** — `keep` / `delete` / `refactor`.
- **justification** — one sentence with `file:line` evidence.
- **canonical** (delete only) — the surviving test that covers
  the same behavior.
- **pinned-detail** (implementation-pinning only) — the production
  site whose internals are being asserted.
- **prod-shim** (when discovered) — a backward-compatibility shim
  in production code that exists solely to keep this test passing.

Target length: 500–1500 words per group. The findings must be
self-contained — the acting phase reads the report, not the test
files.

## Tool Constraints

**Allowed**:
- Read: test files in the group, production code they exercise,
  prior discovery report (INPUT_REPORT)
- Grep: cross-package search for duplicate assertion patterns
- Glob: locating sibling test files
- Write: analysis report at the reserved report path only

**Forbidden**:
- Modifying source files (deletions and refactors are the phase's
  job, not the analysis objective's)
- Launching sub-agents
- Writing outside the reserved report path

## Response Format

After writing the report, return:

```
Wrote test analysis report to <reserved report path>.

Summary: [N] files judged; [D] delete, [R] refactor, [K] keep.
Notable: [one sentence on the highest-impact finding].
```

Do not return the full findings. The report file is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Judging without reading production code | Implementation-pinning signal is invisible | Read the code under test before judging |
| Skipping prod-shim detection | Leaves the worst outcome unflagged | Cross-reference prod code for shims |
| Vague justifications | Phase cannot act confidently | Cite `file:line` for every claim |
| More than 30 files per invocation | Shallow judgments, context-budget miss | Halt and report a bound violation |
| Emitting one finding per test | Reduces signal density | Emit one finding per applicable criterion |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile

## Parent-Layer Dispatch

This objective is dispatched once per discovery group by the
`cleanup_tests_phase` skill. The phase passes the group's file
list as INPUT_REPORT and receives the analysis report path back.

## Manual Invocation Only

This capability is manual-invocation only (intent §3 D2, §5 scope).
No hook, no CI, no schedule.
