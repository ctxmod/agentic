# Reference Validation

Use when validating that cross-references resolve correctly and maintain integrity. Verifies links, inline tags, name patterns, and compulsory reads resolve to existing targets.

Brief template for a sub-agent — Validate reference integrity:

```text
Validate reference integrity for: {scope or artifact description}

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
- `{reference discovery report if available}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-val-references" --owner <work item, if any>` immediately before writing; returns the path
- ARTIFACT_PATHS={paths to validate}
- REFERENCE_TYPE={sak-tags|markdown-links|compulsory-reads|all}

Delimitations:
- Reference validation only
- Do not validate content of referenced files
- Do not modify artifacts

Notes:
- Verify target paths exist for all reference types
- Verify name patterns resolve to files
- Check required vs optional handling
- Validate relative path resolution from source location
- Check anchor references within documents
- Broken required references are Errors
- Broken optional references are Warnings
```
