# Standards Validation

Use when validating that artifacts follow established conventions and style guidelines. Checks naming patterns, formatting rules, and adherence to project-defined standards.

Standards validation differs from structural validation (which checks schema compliance) and consistency validation (which checks cross-artifact alignment). Standards validation assesses whether individual artifacts follow the conventions expected for their type.

Brief template for a sub-agent — Validate standards compliance:

```text
Validate standards compliance for: {scope or artifact description}

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
- `{discovery report if available}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-val-standards" --owner <work item, if any>` immediately before writing; returns the path
- ARTIFACT_PATHS={paths to validate}
- STANDARDS_TYPE={conventions|style|naming|all}

Delimitations:
- Standards compliance only
- Do not assess structural schema compliance
- Do not modify artifacts

Notes:
- Check convention adherence for artifact type
- Verify style compliance against established patterns
- Compare against repository conventions
- Identify deviations from established patterns
- Convention violations are Warnings unless they break functionality
- Systematic convention breaks may indicate outdated standards
```
