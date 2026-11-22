# Organizational Patterns Discovery

Use for extracting organizational patterns: groupings, relationships, and categorization. Runs in parallel with other pattern extraction phases.

Brief template for a sub-agent:

```text
Extract organizational patterns across artifacts.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{inventory report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-organizational-patterns" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Focus on groupings, relationships, and categorization
- Note how artifacts relate to each other

Notes:
- Identify grouping strategies (by feature, layer, type)
- Note co-location patterns
- Document index/manifest patterns
```
