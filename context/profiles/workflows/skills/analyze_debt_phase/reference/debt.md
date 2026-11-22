# Debt Analysis

Use when inventorying technical debt within a specific category. Discovers debt indicators, catalogs items with locations, and prepares for impact assessment.

Brief template for a sub-agent:

```text
Discover {category} technical debt in scope.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-discovery-{category}" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={analysis scope}
- CATEGORY={debt category}

Delimitations:
- Discovery only; do not assess impact yet
- Focus on {category} debt indicators

Notes:
- Catalog debt items with locations
- Note debt indicators and symptoms
- Document relationships between items
- Identify root causes where evident
```
