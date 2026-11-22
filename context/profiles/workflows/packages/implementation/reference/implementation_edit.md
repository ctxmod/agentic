# Edit Implementation

Use when applying targeted edits to existing artifacts. Restricts scope to modifications only--no creation or deletion of artifacts.

Brief template for a sub-agent:

```text
Apply targeted edits to {artifacts} based on plan.

Compulsory reads:
- the `implementation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_implementation.md`
- `{planning report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={planning report path}
- TARGET_PATHS={paths to edit}

Delimitations:
- Edit only; do not create or delete
- Stay within planned scope
```
