# Structural Patterns Discovery

Use for extracting structural patterns: file organization, component layouts, hierarchies. Runs in parallel with other pattern extraction phases.

Brief template for a sub-agent:

```text
Extract structural patterns from artifacts.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{inventory report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Focus on structure: file organization, component layouts, hierarchies
- Note frequency of pattern occurrence

Notes:
- Compare similar artifacts for structural consistency
- Identify dominant vs variant structures
- Note deviations from majority pattern
```
