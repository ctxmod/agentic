---
description: "Acting phase for the tests cleanup capability — runs discovery, fans out per-group analysis objectives, and applies findings as an overlapped pipeline (analyzing group N+1 while applying group N), one validation-gated commit per group, with a cumulative corpus-safety gate and borderline cases flagged for human review."
binds_to_depth: phase
---

# Cleanup Tests Phase

Acting phase for the `tests` cleanup capability. Runs the
discovery/grouping objective once to partition the test corpus,
fans out the per-group analysis objective once per group, and
applies the recommended deletions and refactors as an overlapped
pipeline: while group N+1 is being analyzed (read-only), group
N's findings are applied, validated, and committed.

Tests cleanup is a **phase, not a workflow**. The main agent
invokes it directly via the `/cleanup_tests` slash-command.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Test debt is suspected — suite runtime is growing, tests break
  on legitimate refactors, or backward-compat shims accumulate.
- A periodic sweep is warranted (the operator decides cadence; this
  capability runs on manual invocation only).
- The blast radius of test deletion is acceptable (deleting tests
  cannot break production; worst case is reduced coverage).

**When not to use**:

- The concern is duplicated production code, not tests — that is
  the `code-debt` capability (separate issue).
- The concern is work-store archival — that is `archive_cleanup` (agentic `work` profile).
- A specific test is flaky and you want a one-off fix — use
  `/implement_fix` directly.

## Phase Orchestration

Discovery runs once. After it, analysis and apply run as an
**overlapped pipeline**: per-group analysis is read-only and may
proceed while the previous group's apply round executes, but the
apply rounds themselves are strictly serialized — at most one
round is being applied, validated, and committed at any time.

```
Phase 1: Discovery / Partition
         ↓  (groups of ≤30 test files)
Phase 2 ⟷ Phase 3, overlapped per group:
    analyze group N+1 (read-only)
         ∥ runs while →
    apply group N: delete/refactor → validate → commit
         ↓  (one apply round per group; rounds serialized)
      Complete
```

### Phase 1: Discovery / Partition

Dispatch `cleanup_tests_discover_objective` to enumerate test
files via the repository file index and partition into ≤30-file groups.

- Variant: discover
- Input: `scope` argument (optional)
- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery.md`

The discovery objective is the sole caller of the file-index reads; analysis
objectives consume its manifest rather than re-querying.

### Phase 2: Per-Group Analysis (one dispatch per group)

Fan out **one `cleanup_tests_analyze_objective` per group**. Each
objective receives its group's file list as INPUT_REPORT (a slice of
the discovery manifest).

- Variant: analyze
- Input: discovery report + group ID
- Report per group: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-analysis-{group-id}.md`

**Dispatch discipline**: analysis dispatches are **read-only**
and are the only component permitted to overlap an in-flight
apply round — while group N+1's objective runs, this phase
applies group N's findings. What stays strictly serialized is
the apply side: **at most one apply round in flight** at any
time, even when two groups' findings look disjoint. Concurrent
applies in the same or adjacent packages interleave their scoped
validations and race the git index; serialization is the guard
against both. The ≤30-file bound remains load-bearing under the
pipeline: it caps each analysis agent's context so no single
agent overflows, independent of overlap. If the concurrency
instruction in your own agent-depth instructions ever grants
concurrent dispatch, the Phase 2 analysis fan-out (read-only)
may use it; apply rounds stay serialized regardless.

### Phase 3: Apply (pipelined per group, validation-gated)

Apply each group's findings as soon as that group's analysis
report returns — there is **no long accumulation phase** before
the first apply, and findings are never stockpiled to the end.
Operator directive (2026-09-07): perform suggestions as
judgments are made, in batches, committing after each judged
group — applying the previous group runs in parallel with
analyzing the current one. Per round, for group N:

1. **Apply `delete` findings** — remove the test file or the
   specific test function, as the finding specifies.
2. **Apply `refactor` findings** — only when the refactor is
   mechanical and low-risk (e.g. relaxing an assertion to
   contract level). For non-mechanical refactors, flag the
   finding for human review rather than rewriting.
3. **Flag for human review** — any finding whose action is
   ambiguous, whose prod-shim detection fired, or whose blast
   radius touches code outside the test file. Flagged findings
   — and every prod-shim detection — are **never auto-applied**
   under this pipeline; they accumulate across all rounds into
   the single human-review list.

**Validation gate**: after applying group N's findings, run a
scoped compile-and-unit-test gate run over the touched
packages. The round's commit happens only on a green gate. A
red gate blocks the commit: fix forward or revert that round's
changes before proceeding — never commit untested deletions,
never carry a broken tree into the next round.

**One commit per group**: commit group N's deletions/refactors
immediately after its green validation, before starting the next
apply round. Never batch multiple groups into one commit; never
run two apply rounds concurrently.

**Cumulative corpus-safety gate**: before applying each round's
deletions, compute the delete rate across **all rounds so far,
including this one**, against the corpus. If the cumulative rate
exceeds 20% of the corpus (intent §5 cost-vs-value calibration),
halt and request human review of the applied rounds plus the
pending findings. The gate is evaluated at **every** apply round,
not once at a terminal phase — a pipeline that checks only the
final total can blow past the threshold many rounds before anyone
looks.

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-apply.md`

The apply report carries one section per applied round:

- Files deleted (with the finding that justified each deletion).
- Files refactored (with diff summary).
- Findings flagged for human review (with the reason for flagging).
- Validation result and commit hash for the round.
- Corpus-safety gate status for the round (pass /
  halt-with-cumulative-percentage).

## Information Flow

```
Phase 1: Discovery / Partition
- Receives: {inherited work item context}, scope
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery.md

Phase 2: Per-Group Analysis (one dispatch per group)
- Receives: {inherited}, Phase 1 output, group ID
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-analysis-{group-id}.md
  (one per group)

Phase 3: Apply (pipelined, per group)
- Receives: {inherited}, each Phase 2 output as it lands
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-apply.md
  (one section per applied group) plus one validation-gated
  commit per group
```

Main agent passes report **paths** between phases, never report
content. Each phase reads prior phase report paths as INPUT_REPORTs.

## Manual Invocation Only

This capability is manual-invocation only (intent §3 D2, §5 scope,
§7 AC item 26). **No hook, no CI, no schedule.** The operator runs
`/cleanup_tests` explicitly when test debt is suspected. The
capability must not be wired into any automatic check or
background process — test deletion, while safe at the production
level, reduces coverage and demands a human decision per sweep.

## Error Handling

| Situation | Response |
|-----------|----------|
| Discovery finds zero test files | Report and exit (nothing to do) |
| A group exceeds 30 files | Halt, request re-partition (discovery bug) |
| Cumulative corpus-safety gate trips (>20% at any round) | Halt, request human review of applied rounds plus pending findings |
| A round's validation gate fails | Fix or revert that round's changes before its commit; never proceed on red |
| Prod-shim detection fires | Flag finding for human review; do not auto-delete |
| Apply encounters a non-mechanical refactor | Flag for human review |

Error handling follows stop-report-propose-await pattern.

## Quality Criteria

| Criterion | Validation Method | Phase |
|-----------|-------------------|-------|
| ≤30-file bound honored | Discovery partition + analysis halt-on-violation | 1, 2 |
| All four criteria applied | Each analysis report covers redundancy, implementation-pinning, flakiness, cost-vs-value | 2 |
| Cumulative corpus-safety gate | Every apply round computes the cumulative delete rate, halts above 20% | 3 |
| Validation-gated per-group commits | Green scoped compile+unit check precedes each one-group commit | 3 |
| Prod-shim findings flagged | Every prod-shim detection routes to human review | 3 |
| Manual invocation only | No hook, CI, or schedule wiring | all |

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Collapsing groups to dodge the ≤30-file bound | Keep the bound; it caps each analysis agent's context |
| Auto-applying prod-shim deletions | Always route shim findings to human review |
| Checking the corpus-safety gate only at the end | Evaluate it cumulatively at every apply round |
| Stockpiling all groups before the first apply | Apply each group as its analysis report lands |
| Running two apply rounds concurrently | Serialize applies — overlapping rounds interleave validations and race git |
| Re-walking the filesystem in analysis | Consume the discovery manifest only |
| Reading full reports into the orchestrator | Pass report paths between phases |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
paired objective agents.

Phase 1:

Brief template for the `cleanup_tests_discover_objective` agent — Discover and partition test files into ≤30-file groups:

```text
<ctxmod_activate skill="agentic_workflows_cleanup_tests_discover_objective" />
Variant: discover
Scope: <scope or repo-root>
Report to {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery.md.
```

Phase 2 (one dispatch per group, sequential by default):

Brief template for the `cleanup_tests_analyze_objective` agent — Analyze test group <group-id>:

```text
<ctxmod_activate skill="agentic_workflows_cleanup_tests_analyze_objective" />
Variant: analyze
Input: {discovery-report-path}, group-id <group-id>
Bound: ≤30 files (halt if violated)
Report to {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-analysis-{group-id}.md.
```

Phase 3 (apply) is executed by the phase itself — as each
group's analysis report lands, it applies the deletions and
refactors directly, validates the touched packages, and commits
before the next round. It does not dispatch an objective.

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly
as a "clean up test debt" sub-step. Launch the
`cleanup_tests_phase` agent from any workflow that needs test
sweep capability.

Brief template for the `cleanup_tests_phase` agent — Execute the tests cleanup phase:

```text
<ctxmod_activate skill="agentic_workflows_cleanup_tests_phase" />
Execute the full discover → analyze → apply chain.
Scope: <scope or repo-root>
```

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped guidance for the sub-agents this phase launches:

the `cleanup_tests_discover_objective` skill
the `cleanup_tests_analyze_objective` skill
