# Consistency Validation

Use when validating that artifacts maintain consistent terminology, patterns, and conventions across related files. Checks terminology, formatting, naming, and cross-reference alignment.

Brief template for a sub-agent — Validate cross-artifact consistency:

```text
Validate consistency across: {scope or artifact description}

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
- `{pattern discovery report if available}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-val-consistency" --owner <work item, if any>` immediately before writing; returns the path
- ARTIFACT_PATHS={paths to validate}
- ASPECT={terminology|formatting|naming|alignment|all}

Delimitations:
- Consistency validation only
- Do not assess content correctness
- Do not modify artifacts

Notes:
- Check term spelling consistency
- Verify acronym usage after first definition
- Identify conflicting definitions of same term
- Validate heading capitalization consistency
- Check list marker uniformity
- Verify file naming patterns
- Check that references describe targets accurately
- Inconsistent terminology is Warning
- Contradictory definitions are Errors
```
