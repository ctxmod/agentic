# Artifact Inventory Discovery

Use for cataloging artifacts as preparation for pattern extraction. First phase in pattern discovery workflows.

Brief template for a sub-agent:

```text
Inventory artifacts within scope for pattern analysis.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Inventory only; do not extract patterns yet
- Focus on artifacts likely to exhibit patterns

Notes:
- Group artifacts by type
- Note artifact counts per category
- Identify representative samples
```
