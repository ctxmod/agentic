# Target Resolution Discovery

Use for resolving and understanding the target artifact before searching for consumers. First phase in consumer discovery workflows.

Brief template for a sub-agent:

```text
Resolve target for consumer discovery.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- TARGET={target}
- SCOPE={scope}

Delimitations:
- Resolve target identity only; do not search for consumers yet
- Identify all forms the target may take

Notes:
- Determine target type (file, function, identifier, pattern)
- List all aliases, exports, and variations
- Note search patterns for each form
```
