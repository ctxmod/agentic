# Structural Validation

Use when validating schema compliance only, without assessing content quality. Checks format, organization, and required elements against a structural schema.

Brief template for a sub-agent:

```text
Validate structural compliance of {artifacts}.

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `{relevant schema}`

Read if relevant:

- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/completeness.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/consistency.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/cross_validation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/quality_assurance.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/references.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/standards.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/structural.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-val-structural" --owner <work item, if any>` immediately before writing; returns the path
- ARTIFACT_PATHS={paths to validate}

Delimitations:
- Schema compliance only
- Do not assess content quality
```
