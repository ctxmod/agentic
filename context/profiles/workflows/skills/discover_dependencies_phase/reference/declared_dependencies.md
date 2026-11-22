# Declared Dependencies Discovery

Use for extracting declared dependencies from manifest files. Runs in parallel with other dependency extraction phases.

Brief template for a sub-agent:

```text
Extract declared dependencies from manifests and configurations.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{components report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-declared-deps" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Extract declared dependencies only
- Include version constraints where specified

Notes:
- Parse manifest files (package.json, go.mod, requirements.txt, etc.)
- Note development vs production dependencies
- Include peer and optional dependencies
```
