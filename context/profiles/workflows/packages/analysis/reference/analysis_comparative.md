# Comparative Analysis

Use for creating comparative analysis of alternatives based on research findings. Follows alternative research phases.

Brief template for a sub-agent:

```text
Create comparative analysis of {alternatives} based on research findings.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{criteria-definition report path}`
- `{all alternative research report paths}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Analysis only; do not conduct additional research
- Compare all alternatives against same criteria
- Provide objective assessment without predetermined preference

Notes:
- Create comparison matrix for quantifiable criteria
- Document trade-offs between alternatives
- Note where alternatives are equivalent
- Identify scenarios where each alternative excels
```
