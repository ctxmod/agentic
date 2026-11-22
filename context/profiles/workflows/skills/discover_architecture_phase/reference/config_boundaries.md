# Configuration Boundaries Discovery

Use for identifying configuration separation patterns: environments, features, deployments. Often runs in parallel with boundary detection.

Brief template for a sub-agent:

```text
Identify configuration and environment boundaries.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{scan report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-config-boundaries" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Focus on configuration separation: environments, features, deployments
- Do not assess configuration quality

Notes:
- Identify environment-specific configurations
- Locate shared vs scoped settings
- Note override mechanisms
```
