# Validation Synthesis

Use when producing the final validation report from aggregated findings. Creates executive summary, metrics dashboard, and prioritized remediation recommendations.

Brief template for a sub-agent — Synthesize validation report:

```text
Synthesize validation report from aggregated findings.

Compulsory reads:
- the `writing_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `{aggregation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Synthesis only; do not re-validate or remediate

Notes:
- Provide executive summary with pass/fail status
- Include metrics dashboard (errors, warnings, coverage by dimension)
- Group findings by severity, then by artifact
- Provide prioritized remediation recommendations
- Include dimension-specific summaries
- Note systemic issues requiring architectural attention
```
