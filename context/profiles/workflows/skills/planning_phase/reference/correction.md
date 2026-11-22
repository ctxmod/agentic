# Correction Planning

Use when implementing fixes based on validation findings. Plans minimal corrections that address each finding without expanding scope, sequencing corrections to avoid conflicts.

Brief template for a sub-agent:

```text
Plan corrections for validation findings.

Compulsory reads:
- the `planning_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_plan.md`
- `{findings-resolution report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-correction-plan" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={findings-resolution report path}

Delimitations:
- Plan corrections only; do not apply
- Define minimal changes per finding
- Do not expand scope beyond findings

Notes:
- Sequence corrections to avoid conflicts
- Group related corrections by artifact
- Note corrections that may affect multiple findings
- Prioritize by severity (errors before warnings)
```
