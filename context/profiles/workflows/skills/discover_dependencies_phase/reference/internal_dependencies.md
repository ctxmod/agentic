# Internal Dependencies Discovery

Use for extracting dependencies between components within scope. Runs in parallel with other dependency extraction phases.

Brief template for a sub-agent:

```text
Extract internal dependencies between components.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{components report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Focus on dependencies between components within scope
- Trace imports, references, and invocations

Notes:
- Build import/reference graph
- Identify circular dependencies
- Note coupling strength (tight vs loose)
```
