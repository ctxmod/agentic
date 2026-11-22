# Dependency Graph Construction

Use for constructing a unified dependency graph from multiple dependency extraction phases. Follows parallel dependency extraction.

Brief template for a sub-agent:

```text
Construct unified dependency graph from extracted dependencies.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{declared-deps report path}`
- `{internal-deps report path}`
- `{implicit-deps report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-dependency-graph" --owner <work item, if any>` immediately before writing; returns the path
- DEPTH={depth}

Delimitations:
- Graph construction only; do not analyze health
- Apply depth filter from parameters

Notes:
- Merge all dependency sources
- Resolve transitive dependencies if depth includes them
- Produce adjacency representation
```
