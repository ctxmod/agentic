# Aggregation Analysis

Use when multiple parallel analysis reports require thematic grouping before synthesis. Groups findings by theme rather than by analysis type, identifying correlations and contradictions.

Brief template for a sub-agent:

```text
Aggregate analysis findings across multiple reports by theme.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{report 1 path}`
- `{report 2 path}`
- `{report 3 path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-aggregation" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORTS={comma-separated report paths}

Delimitations:
- Aggregation only; do not synthesize recommendations yet
- Group by theme, not by analysis type

Notes:
- Create thematic categories from findings
- Note where dimensions reinforce each other
- Flag contradictions with source attributions
- Map relationships between findings
```
