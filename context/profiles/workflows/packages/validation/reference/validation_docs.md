# Documentation Validation

Phase for validating documentation against structure, completeness, and standards.

Use when validating documentation against structure, completeness, and writing standards. Combines structure validation, completeness checking, and standards validation into a unified documentation-specific validation phase.

Brief template for a sub-agent:

```text
Validate {documentation type} documentation.

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `ctx:github.com/ctxmod/writing/context::shared::procedures@documentation/reference/structure_validation.md`
- `ctx:github.com/ctxmod/writing/context::shared::procedures@documentation/reference/completeness_check.md`
- `{documentation path}`

Read if relevant:
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/standards.md`
- `ctx:github.com/ctxmod/writing/context::shared::procedures@specification/reference/template_compliance.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-validation-docs" --owner <work item, if any>` immediately before writing; returns the path
- DOC_PATH={documentation to validate}
- VALIDATION_TYPE={structure|completeness|standards|comprehensive}

Delimitations:
- Validate documentation only; do not modify
- Flag violations with locations and severity
- Reference applicable standards in findings
- Do not rewrite content during validation

Notes:
- Use structure-validation procedure for hierarchy and heading checks
- Use completeness-check procedure for required section validation
- Use standards validation for writing quality checks
- Use template-compliance for specification documents
- Comprehensive validation combines all applicable procedures
```

## Related

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_general.md" />
- `ctx:github.com/ctxmod/agentic/context::shared::writing_phase@workflows/reference/docs.md`
- `ctx:github.com/ctxmod/writing/context::shared::procedures@documentation/PACKAGE.md`
