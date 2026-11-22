# General Remediation

Use when addressing a broad set of validation findings. Prioritizes errors over warnings and documents each correction.

Brief template for a sub-agent:

```text
Remediate validation findings in {artifacts}.

Compulsory reads:
- the `implementation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_remediation.md`
- `{validation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-remediation" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={validation report path}
- ARTIFACT_PATHS={paths to remediate}

Delimitations:
- Address errors first, then warnings
- {scope constraints}

Notes:
- Document each correction
- Flag findings that cannot be auto-corrected
```
