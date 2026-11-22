# Validation Loop Pattern

A bounded fix/check cycle driven by deterministic oracles. One agent —
the story-completion validator — runs a check, repairs what failed
in-session, and rechecks; the loop iterates only when a failed oracle
requires another fix/check cycle. It is not a default multi-iteration
critic fan-out: there is no standing iteration budget and no subjective
reviewer.

```
   ┌─────────────────┐
   ↓                 │
Run deterministic    │
oracles              │
   ↓                 │
Oracle failed? ─No──→│ Done: commit with
   ↓ Yes             │      byte identity
Repair in-session    │
   └─────────────────┘
```

## When to Use

Use when:

- Quality must meet deterministic criteria before a story closes
- Failures are detectable by oracles (check-gate exit status, structured
  records, selected package-test and test-tool outcomes), not by
  subjective review verdicts
- Corrections can be applied in-session by the same agent

Do not use the loop to compensate for presumed model weakness on simple
work — reliable instruction following does not need a critic cycle. A
green first pass means zero iterations. Multi-dimension validation
panels are an explicit high-risk choice, never the default.

## Loop Components

### Deterministic oracle check

Scoped checks selected for the changed artifacts: the repository's
check gate scoped to the changed path prefix, plus deliberately
selected package tests (implementation-coupled tests run by name) and
test tools (black-box programs run live against the built binary) only
when they answer a story acceptance criterion or material risk.

- **Input**: story scope (touched plus demonstrably related packages),
  acceptance criteria
- **Output**: pass/fail per oracle with structured evidence

### In-session repair

The same agent fixes every failure in the touched/related scope —
including a proven pre-existing failure, which is never left broken as
"baseline". A failure proven to come solely from an untouched,
unrelated scope is recorded without expanding the commit; it does not
trigger whole-tree cleanup.

### Recheck

Every byte changed after a failed pass is included in a later
successful oracle run before the story is declared done.

## Execution Strategy

The story-completion validator (Haiku-class) owns the whole cycle in
one session:

1. **Derive scope**: touched packages plus demonstrably related ones
   (imports, generated artifacts, configuration ownership, explicit
   story scope), recording why each additional path is related.
2. **Select oracles**: scoped check-gate runs appropriate to the changed
   artifacts; package tests and test tools only when they answer a
   criterion or material risk.
3. **Check**: run the oracles.
4. **Repair**: fix failures in scope, in-session. Never hand the fix to
   another agent — if the cycle cannot complete, return a blocker and
   create no success claim.
5. **Recheck**: repeat check/repair only while an oracle fails.
   Iteration is oracle-driven, not budget-driven: no default iteration
   count, and no iteration at all when the first pass is green.
6. **Commit**: stage by explicit story paths and prove commit-scope
   byte identity (see the ctxmod builtin `commit_discipline` profile and
   the repository's own commit workflow).

REPORTS_BASE derives from work item context per `ctx:github.com/ctxmod/agentic/context::shared::framework@workflows/reference/params.md`; the validator's outcome lands in its completion record, not in per-iteration sub-agent reports.

## Termination Conditions

### Success

Every selected oracle passes after the last change. The success record
carries the commit identifier, scoped check-gate result, deliberately
run package-test result set, deliberately run test-tool result set, and
commit-byte proof.

### Working-claim limit

If no relevant test tool was deliberately exercised, the conclusion may
state that scoped quality gates pass — it must not state that
real-world behavior works.

### Blocker

An oracle that cannot be satisfied in scope, or a repair that cannot
complete: report the blocker, surface it for a decision, and do not
lower the criteria or declare success from prose confidence.

## Success Is Evidence-Backed

The validator reasons about which checks are relevant, but success
comes from deterministic observations — check-gate exit status, structured
records, selected test outcomes, and commit-scope evidence — never from
prose confidence alone.

## Relationship to Other Patterns

- **Sequential → Validation Loop**: a sequential workflow ends in the
  story-completion validation cycle
- **Validation Loop → Sequential**: after the loop completes, the
  workflow proceeds
- No parallel critic fan-out: the loop is one bounded session, not a
  panel of subjective reviewers

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Default multi-iteration critic fan-out | Burns context without changing confidence on simple work | One story-completion session with deterministic oracles |
| Fixed iteration budgets | Iteration should be oracle-driven | Iterate only while an oracle fails |
| Subjective review verdicts as success | Not inspectable | Deterministic observations only |
| Deferring touched-scope failures as "pre-existing" | Leaves the changed area worse than the validation implies | Repair in-session |
| Report-only validation handing fixes across a context boundary | Unresolved state crosses sessions | Same-session ownership of fix and commit |
| Empty acceptance criteria | Undefined success condition | Define the oracle set before checking |
