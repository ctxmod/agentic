# Recommendation Analysis

Use when producing actionable recommendations from analysis or synthesis findings. Generates categorized recommendations with confidence levels, applicable across assessment types: quality verdicts, comparative evaluations, remediation priorities, and approval decisions.

Brief template for a sub-agent:

```text
Generate recommendations from {analysis type} findings.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{analysis or synthesis report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={analysis or synthesis report path}
- RECOMMENDATION_FORMAT={verdict|ranked-list|action-plan}

Delimitations:
- Recommendations only; do not conduct additional analysis
- Include confidence levels for each recommendation
- Provide rationale grounded in findings

Notes:
- For verdict format: classify outcome and list supporting evidence
- For ranked-list format: rank by priority with scenario guidance
- For action-plan format: sequence actions by dependency and impact
- Document assumptions underlying recommendations
- Note risks and mitigation strategies for top recommendations
```
