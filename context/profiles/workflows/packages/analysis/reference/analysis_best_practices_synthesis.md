# Best Practices Synthesis Analysis

Use for synthesizing best practices research findings into actionable recommendations. Final phase in best practices research workflows.

Brief template for a sub-agent:

```text
Synthesize research findings on {topic} best practices into recommendations.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{research-web report path}`
- `{research-community report path}`
- `{research-github report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-synthesis" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Synthesis only; do not conduct additional research
- Resolve contradictions between sources with rationale
- Provide actionable recommendations with supporting evidence

Notes:
- Identify consensus patterns across sources
- Note where sources disagree and why
- Prioritize recommendations by impact and confidence
- Include confidence assessment per recommendation
```
