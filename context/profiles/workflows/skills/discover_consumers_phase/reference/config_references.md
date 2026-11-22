# Configuration References Discovery

Use for searching configuration and metadata references. Runs in parallel with other reference search phases.

Brief template for a sub-agent:

```text
Search for configuration and metadata references.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{target-resolution report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-config-refs" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Configuration files, manifests, build scripts
- Include documentation references

Notes:
- Search configuration files
- Check build and deployment scripts
- Include documentation references
```
