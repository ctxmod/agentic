---
description: "Step-level implementation mechanics — change planning, execution checkpoints, completion reporting, context assessment."
binds_to_depth: procedure
---

# Implementation Procedures

Execute a single, bounded implementation procedure and return its result.
Each procedure below is a self-contained set of prerequisites, steps, and
decision points; the variant material lives in this skill's `reference/`
directory and is read conditionally based on what the assigned task needs.

## Procedure Selection

Read the reference file matching the assigned task. Do not read all four —
each is independently applicable and reading extras wastes context.

| Assigned task | Read |
|---------------|------|
| Sequence modifications, map dependencies, define rollback strategy | `reference/change_planning.md` |
| Confirm context is sufficient before modifying; identify gaps | `reference/context_assessment.md` |
| Execute planned modifications with checkpoint verification | `reference/execution_checkpoints.md` |
| Produce the completion report summarizing what was done | `reference/completion_reporting.md` |

### Change Planning

Establish the order of modifications, identify dependencies between
changes, and define rollback strategy. Produces an execution plan that
minimizes intermediate broken states and enables recovery at any point.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure/reference/change_planning.md" />

### Context Assessment

Confirm the context is sufficient before modifying artifacts. Catches
gaps early — modifying on incomplete context produces rework.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure/reference/context_assessment.md" />

### Execution Checkpoints

Execute planned modifications with checkpoint verification between
logical units. Catches failures early in long modification sequences.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure/reference/execution_checkpoints.md" />

### Completion Reporting

Produce the completion report summarizing what was modified, why, and
whether verification passed. Report persists at the reserved report path
for downstream phases and audit.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure/reference/completion_reporting.md" />

## Execution Discipline

- Execute only the assigned procedure; do not expand into adjacent steps.
- Report results to the caller-provided path; return a summary, not the
  full body.
- Reference findings with `file:line` ranges rather than inlining content.
- Note explicitly when a procedure's prerequisites are unmet rather than
  improvising a substitute.

## Objective Context

The objective these procedures serve, and its per-scope variants:

the `implementation_objective` skill

## Parent-Layer Dispatch

This procedure is dispatched by the shared `implementation_objective`,
which in turn is dispatched by three sibling phase skills in the
implementation capability:

| Phase skill | Procedures typically dispatched |
|-------------|---------------------------------|
| `implement_task_phase` | context-assessment, change-planning, execution-checkpoints, completion-reporting |
| `implement_fix_phase` | context-assessment, change-planning, execution-checkpoints, completion-reporting |
| `implement_refactor_phase` | context-assessment, change-planning, execution-checkpoints, completion-reporting |

Ad-hoc procedure-depth agents may also load this skill for general
implementation step mechanics outside any of the three capability
phase chains.

- the /implement command selection guidance — which implementation
  variant (task, fix, refactor, or ad-hoc) fits the assigned change —
  <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/implement.md" />

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
