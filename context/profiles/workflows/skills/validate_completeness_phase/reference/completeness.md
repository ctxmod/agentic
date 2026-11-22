# Completeness Validation

Use when validating that artifacts contain all required elements and meet coverage expectations. Checks presence of required sections, frontmatter fields, and acceptance criteria mapping.

Brief template for a sub-agent — Validate completeness of required elements:

```text
Validate completeness of required elements for: {scope or artifact description}

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
- `{criteria discovery report}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-val-completeness" --owner <work item, if any>` immediately before writing; returns the path
- ARTIFACT_PATHS={paths to validate}
- CRITERIA_TYPE={sections|frontmatter|acceptance|all}

Delimitations:
- Completeness validation only
- Do not assess content quality
- Do not modify artifacts

Notes:
- Check all required sections exist per schema
- Verify frontmatter field completeness
- Validate acceptance criteria coverage (if work item context)
- Calculate coverage metrics by artifact type
- Missing required elements are Errors
- Low coverage is Warning
```
