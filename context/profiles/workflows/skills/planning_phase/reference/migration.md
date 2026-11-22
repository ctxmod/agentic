# Migration Planning

Use when planning a phased transition between states. Requires gap analysis as input. Produces a migration plan with phases, rollback points, and verification between phases.

Brief template for a sub-agent:

```text
Plan phased migration from current to target state.

Compulsory reads:
- the `planning_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/phased.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/checkpoint_definition.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_plan.md`
- `{gap analysis report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={gap analysis report path}

Delimitations:
- Define rollback points
- Include verification between phases
- Plan for incremental progress

Notes:
- Follow phased planning variant for phase decomposition
- Use dependency-analysis for phase ordering
- Use checkpoint-definition for inter-phase gates
- Document rollback strategy per phase
```
