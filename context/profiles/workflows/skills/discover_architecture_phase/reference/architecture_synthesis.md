# Architecture Synthesis Discovery

Use for synthesizing architecture discovery findings into a consolidated report. Final phase in architecture discovery workflows.

Brief template for a sub-agent:

```text
Synthesize architecture discovery findings into consolidated report.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{scan report path}`
- `{boundaries report path}`
- `{config-boundaries report path}`
- `{layers report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Synthesize findings only; do not recommend changes
- Produce single consolidated report

Notes:
- Organize by architectural dimension
- Include visual diagram (ASCII) of structure
- Highlight notable boundaries and relationships
```
