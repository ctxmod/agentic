# Work Command Selection Guidance

Selection guidance for the `/work` command — the entry point for
orchestrating autonomous work item creation and execution in a
single uninterrupted workflow. This document captures work-item-type
creation behavior, parent resolution, and the context-transition
that distinguishes `/work` from `/run`. Execution lives in the full
workflow (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_full.md" />).

## Distinction from `/run`

`/work` creates a work item **and** executes it. `/run` only executes
an existing work item. Use `/work` when starting from a description
with no prior work item; use `/run` when resuming an existing
identifier.

## Work Item Creation Behavior

`/work [description] [flags]` infers or accepts the work item type
via flags. Parent requirements vary by type.

| Type | Epic Required | Story Required | Behavior |
|------|---------------|----------------|----------|
| `--type epic` | No | No | Creates standalone epic |
| `--type story` | No | No | Creates standalone story (or under epic if `--epic` provided) |
| `--type issue` (default) | No | Yes | Creates issue under story (fails if `--story` not provided) |
| `--type task` | No | Yes | Creates task under issue (fails if parent not resolved) |

**Parent resolution**:

- `--epic {id}` + `--type story` → Create story under epic
- `--story {id}` + `--type issue` → Create issue under story
- Missing required parent → Error with guidance to specify parent flag

## Context Transition

This command transitions from general context to work item context
during execution. It starts without work item context (Phase 0
creates context) and operates within work item context for all
subsequent phases.

| Phase Range | Context | Report Location |
|-------------|---------|-----------------|
| Phase 0-3 | General | the store's top-level `reports/<YYYY-MM>/` (no owner) |
| Phase 4-17 | Work item | `<work item dir>/reports/<YYYY-MM>/` |

Once the work item directory exists after Phase 4, REPORT_BASE
updates from the store's top-level `reports/` (no `--owner`) to `<work item dir>/reports/` for all
subsequent phases. This context transition is the load-bearing
distinction from other commands, which either require work item
context upfront or operate entirely without it.

## Workflow Shape

The full workflow combines work item creation (Phases 0-6) with work
item execution (Phases 7-17), connected by a handoff gate. See
full workflow (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_full.md" />) for complete phase
definitions, conditional logic, parallelization strategy, error
handling, and work item type adaptations.

**Creation Workflow (Phases 0-6)**: Main agent performs Phase 0
directly (input analysis, organization decision, identifier
resolution). Phases 1-3 execute conditionally and sequentially with
sequential decomposition in Phase 2. Phase 4 creates work items
sequentially with per-item validation. Phases 5-6 validate and
remediate schema issues across all created items.

**Handoff Gate**: Validates creation output before execution. Work
item record exists with its body, record state valid, parent references resolve,
creation reports complete.

**Execution Workflow (Phases 7-17)**: Follows the run workflow.
Phase 7 analyzes work item with inherited creation context.
Phases 8-17 execute analysis through completion, conditional on
work item status and validation results.

## Preflight Phase

Main agent performs preflight analysis in Phase 0:

1. **Determine paths**: REPORT_BASE (the owning work item, if any), {date}/{time}, INTENT_PATH
2. **Create intent records**: `ctxmod work new intent "<title>" --owner <work item>
   --body-file <the developer's words>`, then `ctxmod work new criteria
   "<title> intent" --owner <work item>`, and write the developer intent to
   the document's printed `file` (INTENT_PATH)
3. **Discover context**: Invoke workflow reference discovery agent
4. **Prepare parameters**: Assemble parameters for all phases

See <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" /> (preflight phase)
for the preflight pattern that applies to every workflow.

## Decision Guide

| Situation | Recommended Command |
|-----------|---------------------|
| Start work from a description, no prior work item | `/work` |
| Resume an existing work item to completion | `/run` |
| Create a work item without executing it | `/write epic\|story\|issue\|task` |
| Execute a single task with clear acceptance criteria | `/implement task` |

## Execution Pointers

- `/work` always starts in general context and transitions to work
  item context at Phase 4
- The handoff gate between creation and execution is load-bearing —
  it prevents executing against a malformed work item
- After Phase 4, all report paths shift to `<work item dir>/reports/`

See full workflow (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_full.md" />),
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />, and
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`.
