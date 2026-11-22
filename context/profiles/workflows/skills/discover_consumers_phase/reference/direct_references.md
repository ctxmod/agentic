# Direct References Discovery

Use for searching direct references: imports, includes, explicit mentions. Runs in parallel with other reference search phases.

Brief template for a sub-agent:

```text
Search for direct references to target.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{target-resolution report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-direct-refs" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Direct references only: imports, includes, explicit mentions
- Record location with line numbers

Notes:
- Search for each target form from resolution
- Group references by referencing artifact
- Note reference context (import, invocation, type usage)
```
