---
description: "Lifecycle complete phase — verify acceptance criteria and transition work item to terminal completed state through context resolution, parallel completion verification, synthesis analysis, and status transition."
binds_to_depth: phase
---

# Lifecycle Complete Phase

Lifecycle complete phase — verify acceptance criteria and children are
resolved, then transition the work item to the terminal completed
state. This is one of four sibling leaf phase skills in the lifecycle
capability (start, block, status, complete); it dispatches bare per
The only-where-content-exists decision — no objective/procedure
layer.

Lifecycle complete is a **leaf phase**. The main agent invokes it
directly via the absorbed `/lifecycle_complete` slash-command or any
other domain's workflow agent references this phase as a "lifecycle
complete" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Work item execution is finished
- All acceptance/success criteria are satisfied
- Child items are resolved (completed or cancelled)
- Transitioning work items to terminal completed state

The complete phase applies to all work item types with type-specific
criteria verification.

## Phase Orchestration

This leaf phase dispatches Sonnet sub-agents for parallel criteria and
children verification, then a Sonnet fan-in synthesis, then a Haiku
status transition; no objective/procedure layer exists.

Parallel verification for criteria and children, followed by synthesis
analysis:

```
Phase 1: Context Resolution (main agent)
         |
Phase 2a: Build Smoke Gate (AC0, Sonnet) — mandatory, fail-loud
         |  └─ scoped compile gate (non-zero exit halts)
         |
Phase 2b: Completion Verification (parallel, Sonnet) — skipped if 2a fails
         |  ├─ Criteria Verification
         |  └─ Children Verification
         |
Phase 3: Completion Analysis (Sonnet, fan-in) — skipped if 2a fails
         |
Phase 4: Status Transition (Haiku, conditional)
         |
Phase 5: Notification (main agent)
```

### Phase 1: Context Resolution

`ctx:github.com/ctxmod/agentic/context::shared::lifecycle_start_phase@work/reference/context_resolution.md`

**Workflow-specific notes**:
- Main agent resolves work item from parameter, conversation, or directory
- Determines WORK_ITEM_PATH, WORK_ITEM_TYPE, WORK_ITEM_ID
- Validates work item file exists and is accessible
- No sub-agent needed

### Phase 2: Build Smoke Gate (AC0) + Completion Verification

**Phase 2a — Build Smoke (AC0, mandatory hard gate).** Runs BEFORE any
acceptance-criteria verification. The repository's scoped compile gate
(for example `go build ./...`, `cargo check`, or the repository check
gate's compile stage) must exit 0 before criteria or children
verification proceeds. A non-zero exit
halts the phase flow and blocks the transition to `done` (completed). See
`build-refs-fail-loud` rationale below.

The exact build-smoke recipe is repository-specific and lives with the
repository's own context (its build and gate documentation).

- Report: `<work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-completion-build-smoke.md`

**Workflow-specific notes (Phase 2a)**:
- Single Sonnet sub-agent runs the build smoke and reports PASS/FAIL
- **On FAIL**: skip Phase 2b, skip Phase 3, proceed to Phase 5 with the
  failure report (transition blocked, fail-loud). Do NOT attempt to
  verify acceptance criteria against a non-compiling tree.
- **On PASS**: proceed to Phase 2b (parallel criteria + children
  verification)

**Phase 2b — Completion Verification (parallel).** Runs only after the
build smoke exits 0.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::lifecycle_complete_phase/reference/criteria_verification.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::lifecycle_complete_phase/reference/children_verification.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

- Reports: `<work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-completion-criteria.md`,
  `<work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-completion-children.md`

**Workflow-specific notes (Phase 2b)**:
- Two Sonnet sub-agents launched in parallel in a single message
- Each agent verifies different completion aspect
- Both agents receive work item path and type

**`build-refs-fail-loud` rationale.** A verification-theater incident
saw a duplicate constant declaration break a document-renderer
package build for three subsequent commits (three issue verifications)
while every completion report claimed "Go build: Success".
The build smoke gate exists to make that failure mode impossible: the
skill flow cannot reach status transition until a real compilation check
exits zero. Four memories documented the pattern (orchestrators must
verify landed code; harness Edit virtualization; sub-agent
auto-commit; commit validation must scan the full tree for merge
markers).

### Phase 3: Completion Analysis

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::lifecycle_complete_phase/reference/completion_analysis.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

- Report: `<work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-completion-analysis.md`

**Workflow-specific notes**:
- Fan-in synthesis agent that consumes both Phase 2 reports
- Determines completion eligibility based on combined results

### Phase 4: Status Transition (conditional)

`ctx:github.com/ctxmod/agentic/context::shared::lifecycle_start_phase@work/reference/status_transition.md`

**Workflow-specific notes**:
- **Conditional**: Executes only if Phase 3 analysis confirms eligibility
- **Skip**: If analysis finds blocking issues, proceed to Phase 5 with failure report
- Changes status from `in_progress` to `done` (`ctxmod work set <id> status=done`)
- Changes no other field and never the body

### Phase 5: Notification

Main agent returns result to developer. No sub-agent needed.

**Success notification includes**:
- Work item identifier
- Verification summary (criteria met, children resolved)
- New status (completed)
- Impact on parent completion eligibility (if applicable)

**Failure notification includes**:
- Work item identifier
- Unmet acceptance criteria
- Unresolved child items
- Required actions before completion

## Information Flow

```
Phase 1: Context Resolution
- Receives: User parameter or context
- Produces: WORK_ITEM_PATH, WORK_ITEM_TYPE, WORK_ITEM_ID

Phase 2a: Build Smoke (AC0)
- Receives: WORK_ITEM_PATH
- Produces: <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-completion-build-smoke.md
            (PASS = exit 0, FAIL = non-zero exit)

Phase 2b: Completion Verification (Parallel) — skipped if 2a fails
- Receives: WORK_ITEM_PATH, WORK_ITEM_TYPE
- Produces: <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-completion-criteria.md
            <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-completion-children.md

Phase 3: Completion Analysis — skipped if 2a fails
- Receives: WORK_ITEM_PATH, CRITERIA_REPORT, CHILDREN_REPORT
- Produces: <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-completion-analysis.md

Phase 4: Status Transition
- Receives: WORK_ITEM_PATH, COMPLETION_ANALYSIS_REPORT, TARGET_STATUS=completed
- Produces: Modified {WORK_ITEM_PATH}/{TYPE}.md

Phase 5: Notification
- Receives: Transition result, completion analysis report (or build-smoke failure report)
- Produces: Summary response to developer
```

Main agent passes report paths between phases, never report content.

## Conditional Phase Logic

| Phase | Executes When | Skips When |
|-------|---------------|------------|
| Phase 2a (build smoke) | Always | Never |
| Phase 2b (verification) | Build smoke exited 0 | Build smoke exited non-zero |
| Phase 3 | Build smoke exited 0 | Build smoke exited non-zero |
| Phase 4 | Analysis confirms eligibility | Analysis finds blocking issues, OR build smoke failed |

Phases 1 and 5 always execute.

## Workflow Variants

### Direct Completion

Complete without explicit parameter (resolves from context):

```
/lifecycle_complete
```

Main agent resolves work item from active conversation or current directory.

### Explicit Completion

Complete with explicit work item identifier:

```
/lifecycle_complete mnbvcxzqwr
```

Main agent uses provided identifier directly.

### Hierarchical Completion

Completing a parent item after all children are resolved:

```
/lifecycle_complete kpqrstbcdf  # a story whose issues are all done
```

Phase 2 children verification confirms all children resolved, enabling
completion.

## State Transitions

| From | To | Criteria |
|------|-----|----------|
| active | completed | All acceptance criteria met, all children resolved |

Completing from other states (draft, blocked, completed, cancelled) is
invalid. Return error with current state.

## Error Handling

| Situation | Response | Recovery |
|-----------|----------|----------|
| Context resolution fails | Halt, report ambiguity | Specify work item explicitly |
| Build smoke (AC0) exits non-zero | Skip Phases 2b, 3, 4; report build failure | Fix the build break, re-invoke |
| Criteria verification finds unmet criteria | Skip Phase 4, report failures | Address criteria, re-invoke |
| Children verification finds unresolved items | Skip Phase 4, report failures | Resolve children, re-invoke |
| Status transition fails | Halt, report failure | Manual correction, re-invoke |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping the build smoke (AC0) | Verification theater — claiming "build success" without running it | Always run the scoped compile gate first |
| Asserting build success from prior reports | Stale claim masks real break | Run the build fresh and capture the real exit code |
| Completing without verification | Invalid completion | Always verify criteria and children |
| Proceeding after verification failure | Invalid transition | Skip Phase 4, report failures |
| Sequential verification | Wastes turns | Launch Phase 2b agents in parallel |
| Reading intermediate reports | Context bloat | Pass report paths only |
| Completing with active children | Hierarchy inconsistency | Verify children resolved first |

## Cross-Cutting Reference

This skill links cross-cutting principles at their existing `reference/`
paths per intent §4.3 — they are not duplicated here:

- main-agent posture — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />
- orchestration context shape — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
- parameter passing conventions — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
- parallel sub-agent pattern — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
- fan-in synthesis pattern — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
- work-item discovery via the ctxmod work store — `work_store_first` in the ctxmod builtin `work_discipline` profile

## Sibling Phase Reference

The other three lifecycle leaf phase skills:

the `lifecycle_start_phase` skill
the `lifecycle_block_phase` skill
the `lifecycle_status_phase` skill
