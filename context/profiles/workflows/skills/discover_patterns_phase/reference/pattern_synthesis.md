# Pattern Synthesis Discovery

Use for synthesizing pattern discovery findings into a unified report. Final phase in pattern discovery workflows.

Brief template for a sub-agent:

```text
Synthesize pattern discovery findings.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{structural-patterns report path}`
- `{naming-patterns report path}`
- `{organizational-patterns report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-patterns" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Synthesize findings only; do not recommend changes
- Produce single consolidated report

Notes:
- Rank patterns by prevalence
- Highlight cross-cutting patterns
- Note inconsistencies for potential standardization
```
