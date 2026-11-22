# Gap Analysis

Use when comparing current state against a target specification. Identifies what must change without proposing solutions--solution design belongs to the planning phase.

Brief template for a sub-agent:

```text
Identify gaps between current state and target requirements.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{discovery report path}`
- `{target specification path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={discovery report path}
- TARGET_SPEC={target specification path}

Delimitations:
- Compare current to target only
- Do not propose solutions (planning phase handles this)
```
