# Component Identification Discovery

Use for identifying components and their boundaries as preparation for dependency mapping. First phase in dependency discovery workflows.

Brief template for a sub-agent:

```text
Identify components for dependency mapping.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-components" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Identify components only; do not map dependencies yet
- Focus on units that can have dependencies

Notes:
- Define component boundaries
- Note component entry points
- Identify manifest or configuration files
```
