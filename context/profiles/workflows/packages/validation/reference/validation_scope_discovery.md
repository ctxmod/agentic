# Validation Scope Discovery

Use when validation requires understanding the scope and applicable criteria before assessment. Catalogs artifacts, identifies governing schemas, and determines validation criteria per artifact type.

Brief template for a sub-agent — Discover validation scope and criteria:

```text
Discover validation scope and applicable criteria for: {path or scope description}

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={path or work item scope}
- WORK_ITEM_PATH={work item path if context available}

Delimitations:
- Discovery only; do not validate
- Identify all artifacts within scope
- Determine applicable validation criteria per artifact type

Notes:
- Catalog artifacts by type
- Identify governing schemas for each artifact type
- Extract acceptance criteria from work item (if context provided)
- Note cross-artifact relationships for consistency validation
```
