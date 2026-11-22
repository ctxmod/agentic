# Consumer Synthesis Discovery

Use for synthesizing consumer discovery findings into a final report. Final phase in consumer discovery workflows.

Brief template for a sub-agent:

```text
Synthesize consumer discovery into final report.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{target-resolution report path}`
- `{consumer-classification report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-consumers" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Synthesize findings only; do not recommend changes
- Produce single consolidated report

Notes:
- Summarize total consumer count by category
- Include consumer dependency tree
- Highlight tightly coupled consumers
- List all consumer locations with line references
```
