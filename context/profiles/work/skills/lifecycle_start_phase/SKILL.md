---
description: "Lifecycle start phase — begin work on a work item through context resolution, transition verification, and status transition to in_progress."
binds_to_depth: phase
---

# Lifecycle Start Phase

Lifecycle start phase — begin work on an open or blocked work item,
verifying readiness and claiming it in status in_progress. This is one of
four sibling leaf phase skills in the lifecycle capability (start,
block, status, complete); it dispatches bare per the only-where-content-exists
decision — no objective/procedure layer.

Lifecycle start is a **leaf phase**. The main agent invokes it directly
via the absorbed `/lifecycle_start` slash-command or any other domain's
workflow agent references this phase as a "lifecycle start" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Beginning work on an open work item
- Resuming work after resolving a blocker
- Transitioning work items from open or blocked to in_progress
- Enabling child work items to proceed (parent must be in progress)

The start phase applies to all work item types with consistent
verification criteria.

## Phase Orchestration

This leaf phase dispatches Haiku sub-agents for verification and
status transition; no objective/procedure layer exists.

```
Phase 1: Context Resolution (main agent)
         |
Phase 2: Transition Verification (Haiku)
         |
Phase 3: Status Transition (Haiku, conditional)
         |
Phase 4: Notification (main agent)
```

### Phase 1: Context Resolution

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::lifecycle_start_phase/reference/context_resolution.md" />

**Workflow-specific notes**:
- Main agent resolves work item from parameter, conversation, or directory
- Determines WORK_ITEM_PATH, WORK_ITEM_TYPE, WORK_ITEM_ID
- Validates work item file exists and is accessible
- No sub-agent needed

### Phase 2: Transition Verification

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::lifecycle_start_phase/reference/transition_verification.md" />

- Report: `<work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-start-verification.md`

**Workflow-specific notes**:
- Haiku sub-agent verifies work item can transition to in_progress
- Checks transition-specific criteria based on current status:
  - **open → in_progress**: Required sections present, parent in progress or done
  - **blocked → in_progress**: Blocker resolved, resolution documented
- Produces verification report with eligibility determination

### Phase 3: Status Transition (conditional)

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::lifecycle_start_phase/reference/status_transition.md" />

**Workflow-specific notes**:
- **Conditional**: Executes only if Phase 2 verification passes
- **Skip**: If verification fails, proceed to Phase 4 with failure report
- Haiku sub-agent sets the record's status (`ctxmod work set`)
- Parent status automation: If parent is `open` (draft), transition parent to `in_progress` (active) first
- Changes status from `open` (draft) or `blocked` to `in_progress` (`ctxmod work set <id> status=in_progress`)
- Changes no other field and never the body

### Phase 4: Notification

Main agent returns result to developer. No sub-agent needed.

**Success notification includes**:
- Work item identifier
- Previous status
- New status (in_progress)
- Any warnings from verification

**Failure notification includes**:
- Work item identifier
- Current status
- Verification failure reasons
- Required corrections before starting

## Information Flow

```
Phase 1: Context Resolution
- Receives: User parameter or context
- Produces: WORK_ITEM_PATH, WORK_ITEM_TYPE, WORK_ITEM_ID

Phase 2: Transition Verification
- Receives: WORK_ITEM_PATH, TRANSITION_TYPE=open-to-in_progress|blocked-to-in_progress
- Produces: <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-start-verification.md

Phase 3: Status Transition
- Receives: WORK_ITEM_PATH, VERIFICATION_REPORT, TARGET_STATUS=in_progress
- Produces: The item claimed in status in_progress (`ctxmod work claim --status`)

Phase 4: Notification
- Receives: Transition result, verification report
- Produces: Summary response to developer
```

Main agent passes report paths between phases, never report content.

## Conditional Phase Logic

| Phase | Executes When | Skips When |
|-------|---------------|------------|
| Phase 2 | Always | Never |
| Phase 3 | Verification passes | Verification fails |

Phases 1 and 4 always execute.

## Workflow Variants

### Direct Start

Start without explicit parameter (resolves from context):

```
/lifecycle_start
```

Main agent resolves work item from active conversation or current directory.

### Explicit Start

Start with explicit work item identifier:

```
/lifecycle_start mnbvcxzqwr
```

Main agent uses provided identifier directly.

### Parent Cascade Start

Starting a child item whose parent is still open:

```
/lifecycle_start kpqrstbcdf  # a story whose parent epic vdwgsgzfrs is open (draft)
```

Main agent:
1. Verifies the story's eligibility
2. Discovers the parent epic is `open` (draft)
3. Transitions the epic (`open` → `in_progress`) first
4. Transitions the story (`open` → `in_progress`) second
5. Reports both transitions in notification

## State Transitions

| From | To | Criteria |
|------|-----|----------|
| open | in_progress | Required sections present, parent in progress or done |
| blocked | in_progress | Blocker resolved, resolution documented |

Starting from other states (in_progress, done, cancelled) is invalid.
Return error with current state.

## Error Handling

| Situation | Response | Recovery |
|-----------|----------|----------|
| Context resolution fails | Halt, report ambiguity | Specify work item explicitly |
| Verification fails | Skip Phase 3, report failures | Address criteria, re-invoke |
| Parent transition fails | Halt, report parent issue | Fix parent, re-invoke |
| Status transition fails | Halt, report failure | Manual correction, re-invoke |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Starting without verification | Invalid state transitions | Always verify eligibility |
| Skipping parent automation | Hierarchy inconsistency | Check and transition parents first |
| Proceeding after verification failure | Invalid transition | Skip Phase 3, report failures |
| Reading intermediate reports | Context bloat | Pass report paths only |

## Cross-Cutting Reference

This skill links cross-cutting principles at their existing `reference/`
paths per intent §4.3 — they are not duplicated here:

- main-agent posture — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />
- orchestration context shape — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
- parameter passing conventions — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
- work-item discovery via the ctxmod work store — `work_store_first` in the ctxmod builtin `work_discipline` profile

## Sibling Phase Reference

The other three lifecycle leaf phase skills:

the `lifecycle_block_phase` skill
the `lifecycle_status_phase` skill
the `lifecycle_complete_phase` skill
