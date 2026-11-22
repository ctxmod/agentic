# Creation Implementation

Use when creating new artifacts following plan specifications and schema requirements. Uses Opus for higher accuracy in schema-compliant creation.

Brief template for a sub-agent (model: opus):

```text
Create new {artifact type} following plan specifications.

Compulsory reads:
- the `implementation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_implementation.md`
- `{planning report path}`
- `{relevant schema for artifact type}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={planning report path}
- OUTPUT_PATH={where to create artifact}

Delimitations:
- Create only specified artifacts
- Follow schema exactly
```

## Model Override

This variant uses Opus instead of the phase default. Creation requires careful schema compliance and higher reasoning capability.
