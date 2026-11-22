# General Validation

Use when validating artifacts against a single set of criteria. Suitable for most validation needs where multi-criteria parallel validation is unnecessary.

Brief template for a sub-agent:

```text
Validate {artifacts} against {criteria}.

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `{criteria/schema to validate against}`

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
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-validation" --owner <work item, if any>` immediately before writing; returns the path
- ARTIFACT_PATHS={paths to validate}
- CRITERIA={validation criteria reference}

Delimitations:
- Validate only; do not modify
- {specific criteria focus}

Notes:
- Report all findings by severity
- Identify patterns in violations
```
