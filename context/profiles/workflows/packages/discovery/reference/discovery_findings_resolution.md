# Findings Resolution

Use when implementing corrections based on validation report findings. Loads the validation report, parses findings, filters by severity threshold, and groups findings by affected artifact for systematic correction planning.

Brief template for a sub-agent:

```text
Load validation findings and prepare correction plan.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{INPUT_REPORT or most recent validation report}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-findings-resolution" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={validation report path}
- MIN_SEVERITY={error|warning|info}

Delimitations:
- Parse findings only; do not apply corrections
- Filter by minimum severity threshold
- Do not propose solutions (planning phase handles this)

Notes:
- Group findings by affected artifact
- Identify dependencies between fixes
- Note findings that may interact
- Catalog finding IDs for traceability
```
