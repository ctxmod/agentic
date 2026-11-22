# Complex Remediation

Use when findings require deep understanding of artifact structure or when corrections have non-obvious implications. Uses Opus for higher reasoning capability.

Brief template for a sub-agent (model: opus):

```text
Remediate complex validation findings requiring judgment.

Compulsory reads:
- the `implementation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_remediation.md`
- `{validation report path}`
- `{relevant context files}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-remediation" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={validation report path}
- ARTIFACT_PATHS={paths}

Delimitations:
- Prioritize correctness over speed
- Document rationale for each change

Notes:
- Some findings may require architectural changes
- Flag any findings that need user decision
```

## Model Override

This variant uses Opus instead of the phase default (Sonnet). Complex remediation requires deep reasoning about artifact structure and the implications of corrections.
