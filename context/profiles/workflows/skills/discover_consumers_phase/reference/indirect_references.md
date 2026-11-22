# Indirect References Discovery

Use for searching indirect references: re-exports, aliases, wrappers. Runs in parallel with other reference search phases.

Brief template for a sub-agent:

```text
Search for indirect references to target.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{target-resolution report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-indirect-refs" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Indirect references: re-exports, aliases, wrappers
- Track transitive consumption chains

Notes:
- Find re-exports of target
- Identify wrapper or facade patterns
- Trace alias chains
```
