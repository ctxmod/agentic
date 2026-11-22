# Dependency Synthesis Discovery

Use for synthesizing dependency discovery findings into a final report. Final phase in dependency discovery workflows.

Brief template for a sub-agent:

```text
Synthesize dependency discovery into final report.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{dependency-graph report path}`
- `{components report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Synthesize findings only; do not recommend changes
- Produce single consolidated report

Notes:
- Include ASCII dependency diagram
- Highlight highly connected components
- Note isolated components
- List external dependencies with versions
```
