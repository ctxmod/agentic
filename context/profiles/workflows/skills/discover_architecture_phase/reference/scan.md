# Scan Discovery

Use for broad scanning of directory structure and organizational patterns. Typically the first phase in architecture discovery workflows.

Brief template for a sub-agent:

```text
Scan structure and identify organizational patterns.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Scan only; do not interpret significance
- Catalog directory hierarchy, file types, and groupings

Notes:
- Note naming conventions
- Identify separation patterns (by concern, layer, feature)
- Record file counts per directory
```
