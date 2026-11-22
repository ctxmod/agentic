# Layer Mapping Discovery

Use for mapping architectural layers and their relationships. Typically follows boundary detection phases.

Brief template for a sub-agent:

```text
Map architectural layers and inter-layer relationships.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{boundaries report path}`
- `{config-boundaries report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-layers" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Map relationships only; do not evaluate architecture
- Document observable layers regardless of naming

Notes:
- Identify layer patterns (presentation, domain, infrastructure)
- Note cross-layer dependencies
- Catalog shared utilities
```
