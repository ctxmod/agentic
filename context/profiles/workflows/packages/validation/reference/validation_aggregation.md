# Validation Aggregation

Use when multiple validation phases have completed and findings need aggregation before synthesis. Deduplicates findings, identifies patterns, and calculates aggregate metrics.

Brief template for a sub-agent — Aggregate validation findings:

```text
Aggregate validation findings across multiple dimensions.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{paths to validation reports from all dimensions}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-aggregation" --owner <work item, if any>` immediately before writing; returns the path
- VALIDATION_REPORTS={comma-separated list of validation report paths}

Delimitations:
- Aggregation only; do not re-validate
- Group findings by artifact, then by dimension
- Identify patterns across artifacts

Notes:
- Deduplicate overlapping findings
- Identify systemic issues affecting multiple artifacts
- Calculate aggregate metrics (error count, warning count, coverage)
- Note artifacts with multiple dimension failures
- Identify quick wins (high-impact, low-effort fixes)
```
