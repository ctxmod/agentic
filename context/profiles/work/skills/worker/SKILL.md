---
description: "Orchestrate autonomous work item creation and execution through phased workflows — discovery, planning, creation, validation, implementation, and completion."
binds_to_depth: main...workflow
---

# Work

Orchestrate sub-agents to create work items and execute them autonomously
through discovery, planning, creation, validation, implementation, and
completion verification. The `worker` agent is a **workflow-depth
orchestrator**: it executes phases within the work workflow, dispatching
phase-depth sub-agents for each step and chaining report paths between
phases.

## Main Agent Mantra

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

**Coordinate, don't execute directly** — sub-agents read files, write
reports, and modify artifacts. The worker dispatches tasks, passes
parameters, and chains outputs between phases. **Pass report paths, not
content** — when a sub-agent completes, receive its summary and report
path; pass that path to subsequent sub-agents. **Dispatch sequentially by
default** — use one sub-agent at a time, in the foreground, so no two
agents conflict and each inherits the previous one's context. Concurrent
dispatch is available only if your own agent-depth instructions grants it;
that instruction is authoritative and overrides anything read here.

Work item execution is always sequential: issues within stories and tasks
within issues execute ONE AT A TIME. Each work item receives all previous
work item reports as compulsory reads before execution.

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/patterns_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflows_overview.md" />

## Usage

`/work [description] [flags]`

| Argument | Required | Description |
|----------|----------|-------------|
| `description` | Yes | Brief description of work to create and execute |
| `--epic {id}` | No | Parent epic (id, prefix or directory) for story creation |
| `--story {id}` | No | Parent story (id, prefix or directory) for issue/task creation |
| `--type {type}` | No | Work item type override (epic, story, issue, task) |
| `--id {id}` | No | An existing work item to use instead of creating one |

**Work item creation behavior**:

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

- the /work command selection guidance — when create-and-execute fits
  versus the narrower entry points — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/work.md" />
- the /run command selection guidance — resuming an existing work item
  to completion, with conditional phase logic and error recovery —
  <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/run.md" />

## Work Item Context

This workflow transitions from general context to work item context
during execution. It starts without work item context (Phase 0 creates
context) and operates within work item context for all subsequent phases.

| Phase Range | Context | Report Location |
|-------------|---------|-----------------|
| Phase 0-3 | General | the store's top-level `reports/<YYYY-MM>/` (no owner) |
| Phase 4-17 | Work item | `<work item dir>/reports/<YYYY-MM>/` |

Once the work item directory exists after Phase 4, REPORT_BASE updates
from the store's top-level `reports/` (no `--owner`) to `<work item dir>/reports/` for all subsequent
phases. This context transition distinguishes the full workflow from
commands that require pre-existing work item context.

## Workflow

### Phase 0: Preflight

Main agent performs preflight analysis directly:

1. **Determine paths**: REPORT_BASE (the owning work item, if any), {date}/{time}, INTENT_PATH
2. **Create intent records**: `ctxmod work new intent "<title>" --owner <work item>
   --body-file <the developer's words>`, then `ctxmod work new criteria
   "<title> intent" --owner <work item>`, and write the developer intent to
   the document's printed `file` (INTENT_PATH)
3. **Discover context**: Invoke workflow reference discovery agent
4. **Prepare parameters**: Assemble parameters for all phases

See <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" /> (preflight phase).

The full workflow combines work item creation (Phases 0-6) with work
item execution (Phases 7-17), connected by a handoff gate.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_full.md" />

### Creation Workflow (Phases 0-6)

Main agent performs Phase 0 directly (input analysis, organization
decision, identifier reservation via `ctxmod work new`, one call
per item). Phases 1-3 execute conditionally and
sequentially with sequential decomposition in Phase 2. Phase 4 creates
work items sequentially with per-item validation. Phases 5-6 validate
and remediate schema issues across all created items.

| Phase | Purpose |
|-------|---------|
| 0 | Input analysis, organization decision, identifier reservation via `ctxmod work new` (main agent, direct) |
| 1a | Organization analysis (conditional: `organization_strategy == new`) |
| 1b | Existing parent discovery (conditional: `organization_strategy == append_existing`) |
| 2 | Work decomposition (parallel fan-out, conditional) |
| 3 | Creation planning (fan-in synthesis) |
| 4 | Parallel work item creation (Opus writers, sequential batches) |
| 5 | Schema validation across all created items |
| 6 | Creation remediation loop (conditional, max 3 iterations) |

Identifier reservation in Phase 0 uses `ctxmod work new` (for
work-item IDs) and `ctxmod work new` (for artifact/report paths) as
the canonical collision-safe mechanism. Manual filesystem ID resolution
is prohibited — it races under concurrent sessions. See the
reserve-before-create rule for the full mandate.

### Handoff Gate

Validates creation output before execution begins — the boundary between
creation and execution context. Work item record exists with its body, record state valid,
parent references resolve, all creation reports complete. On failure:
return to Phase 6 or halt.

### Execution Workflow (Phases 7-17)

Follows the run workflow. Phase 7 analyzes work item with inherited
creation context. Phases 8-17 execute analysis through completion,
conditional on work item status and validation results.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_run.md" />

See the full workflow reference for complete phase definitions,
conditional logic, parallelization strategy, error handling, and work
item type adaptations.

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile
`reserve_before_create` in the ctxmod builtin `work_discipline` profile
`memory_management` in the ctxmod builtin `work_discipline` profile
