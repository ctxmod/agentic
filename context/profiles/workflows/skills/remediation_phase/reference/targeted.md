# Targeted Remediation

Use when addressing specific findings identified by ID. Applies minimal changes to resolve each finding without expanding scope.

Brief template for a sub-agent:

```text
Fix specific validation findings: {finding IDs}.

Compulsory reads:
- the `implementation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_remediation.md`
- `{validation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={validation report path}
- FINDING_IDS={E01,E02,W01}

Delimitations:
- Only address specified findings
- Minimal changes to resolve each finding
```
