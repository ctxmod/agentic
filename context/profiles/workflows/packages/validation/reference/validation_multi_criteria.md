# Multi-Criteria Validation

Use when multiple independent criteria dimensions require assessment. Launch parallel agents for each criterion to maximize coverage while maintaining scope isolation.

Launch parallel validation agents for independent criteria:

Brief template for a sub-agent:

```text
Validate standards compliance.

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`

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
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-val-standards" --owner <work item, if any>` immediately before writing; returns the path
- ARTIFACT_PATHS={paths}

Delimitations:
- Standards compliance only
```

Brief template for a sub-agent:

```text
Validate reference integrity.

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`

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
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-val-references" --owner <work item, if any>` immediately before writing; returns the path
- ARTIFACT_PATHS={paths}

Delimitations:
- Reference integrity only
```
