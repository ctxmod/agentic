# Multi-Option Planning

Use when multiple viable approaches exist and selection requires explicit comparison. Presents 2-3 options with trade-offs and recommends one with rationale.

Brief template for a sub-agent:

```text
Evaluate approaches for {objective} and recommend one.

Compulsory reads:
- the `planning_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/general.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/approach_selection.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_plan.md`
- `{analysis report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-planning-options" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={analysis report path}

Delimitations:
- Present 2-3 viable approaches
- Compare trade-offs explicitly
- Recommend one with rationale

Notes:
- Follow approach-selection procedure for evaluating alternatives
- Document scoring criteria and trade-offs
- Include rejection rationale for non-recommended approaches
```
