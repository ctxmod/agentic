# Implicit Dependencies Discovery

Use for extracting dependencies not declared in manifests. Runs in parallel with other dependency extraction phases.

Brief template for a sub-agent:

```text
Extract runtime and implicit dependencies.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{components report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Focus on dependencies not declared in manifests
- Include environment, configuration, and service dependencies

Notes:
- Identify environment variables used
- Note external service references
- Catalog file system dependencies
```
