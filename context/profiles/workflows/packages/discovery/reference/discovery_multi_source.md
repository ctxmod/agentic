# Multi-Source Discovery

Use when multiple non-overlapping domains require investigation in parallel. Launch one agent per domain to maximize coverage while maintaining scope isolation.

Launch multiple discovery agents for non-overlapping domains:

Brief template for a sub-agent:

```text
Discover {domain A} patterns and conventions.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-discovery-{domain-a}" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={domain A scope}

Delimitations:
- Only examine {domain A}
- Do not modify any files
```

Brief template for a sub-agent:

```text
Discover {domain B} patterns and conventions.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-discovery-{domain-b}" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={domain B scope}

Delimitations:
- Only examine {domain B}
- Do not modify any files
```
