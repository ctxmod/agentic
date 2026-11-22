# Prioritization Analysis

Use when prioritizing gaps, issues, or debt items for remediation. Applies impact/effort analysis, considers dependencies, and recommends execution sequence.

Brief template for a sub-agent:

```text
Prioritize identified items for remediation planning.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{assessment or gap-analysis report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={assessment or gap-analysis report path}

Delimitations:
- Prioritize items only; do not define solutions
- Sequencing for impact, not implementation details

Notes:
- Apply impact/effort matrix
- Consider dependencies between items
- Identify quick wins vs strategic investments
- Recommend remediation sequence with rationale
```
