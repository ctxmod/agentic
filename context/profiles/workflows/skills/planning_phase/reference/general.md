# General Planning

Use when planning a single approach based on analysis output. Produces an execution plan with steps, dependencies, success criteria, and risk mitigations.

Brief template for a sub-agent:

```text
Plan approach for {objective} based on analysis findings.

Compulsory reads:
- the `planning_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/general.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_plan.md`
- `{INPUT_REPORT from analysis phase}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={analysis report path}

Delimitations:
- Plan only; do not execute
- {scope constraints}

Notes:
- Follow planning objective procedures
- Include success criteria
- Identify risks and mitigations
- Define checkpoints
- Reference planning procedures as needed (approach-selection, dependency-analysis, checkpoint-definition, risk-assessment, constraint-mapping)
```
