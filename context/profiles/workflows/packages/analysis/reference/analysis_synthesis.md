# Synthesis Analysis

Use when multiple discovery or research reports require aggregation into a unified analysis. Identifies cross-cutting patterns and contradictions between reports.

Brief template for a sub-agent:

```text
Synthesize findings from multiple discovery reports into unified analysis.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{discovery report 1 path}`
- `{discovery report 2 path}`
- `{discovery report 3 path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-synthesis" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORTS={comma-separated report paths}

Delimitations:
- Identify cross-cutting patterns
- Note contradictions between reports
```
