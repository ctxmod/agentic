# Quality Assessment

Use when evaluating artifacts against a specific quality dimension. Applies dimension-specific criteria to score artifacts and identify patterns across multiple items.

Brief template for a sub-agent:

```text
Assess {dimension} quality for artifacts in scope.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{discovery report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- DIMENSION={quality dimension}
- ARTIFACT_PATHS={artifacts from discovery}

Delimitations:
- Assess {dimension} quality only
- Other dimensions handled by separate assessment phases

Notes:
- Apply dimension-specific criteria consistently
- Score artifacts on dimension scale
- Identify patterns across artifacts
- Document issues with specific locations
```
