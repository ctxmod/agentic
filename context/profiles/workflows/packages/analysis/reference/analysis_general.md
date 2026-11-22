# General Analysis

Use when interpreting discovery findings to produce structured recommendations. Suitable for single-source analysis where synthesis across multiple reports is not required.

Brief template for a sub-agent:

```text
Analyze {discovery findings} to {produce recommendations / identify gaps / synthesize sources}.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{INPUT_REPORT from discovery phase}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={prior phase report path}

Delimitations:
- {analysis focus}
- Do not modify any files

Notes:
- {specific patterns to look for}
- {known constraints}
```
