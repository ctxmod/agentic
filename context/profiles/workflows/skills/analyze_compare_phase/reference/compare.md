# Comparative Analysis

Use when evaluating multiple alternatives against consistent criteria to support decision-making. Produces comparison matrix with scores, trade-offs, and context-dependent recommendations.

Brief template for a sub-agent:

```text
Evaluate {alternative} against defined criteria for comparative analysis.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{discovery report for this alternative}`
- `{criteria report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- ALTERNATIVE={alternative name or identifier}
- CRITERIA_REPORT={criteria report path}

Delimitations:
- Evaluate against criteria only
- Do not compare with other alternatives yet (synthesis phase handles this)

Notes:
- Score each criterion with evidence
- Document confidence level per criterion
- Note context-dependent advantages or disadvantages
```
