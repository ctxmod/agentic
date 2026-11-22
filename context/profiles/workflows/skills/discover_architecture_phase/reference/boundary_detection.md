# Boundary Detection

Use for identifying component boundaries, interfaces, and public vs internal elements. Typically follows scan phases in architecture discovery workflows.

Brief template for a sub-agent:

```text
Identify component boundaries and interfaces.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{scan report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Focus on boundary markers: entry points, exports, interfaces
- Do not assess boundary quality

Notes:
- Identify public vs internal elements
- Locate cross-boundary references
- Note boundary enforcement mechanisms
```
