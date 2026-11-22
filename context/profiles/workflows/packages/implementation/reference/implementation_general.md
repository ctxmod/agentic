# General Execution

Use when applying a planned set of changes to artifacts. Suitable for most execution needs where specialized edit, creation, or parallel patterns are unnecessary.

Brief template for a sub-agent — Apply a planned set of changes to artifacts:

```text
{Brief description of what to implement/create}

Compulsory reads:
- the `implementation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_implementation.md`
- `{INPUT_REPORT from planning phase}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={planning report path}
- TARGET_PATHS={artifacts to modify or create}

Delimitations:
- {scope boundaries}
- Only modify specified targets

Notes:
- {special handling}
- {known constraints}
```
