# Naming Patterns Discovery

Use for extracting naming patterns and conventions. Runs in parallel with other pattern extraction phases.

Brief template for a sub-agent:

```text
Extract naming patterns and conventions.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{inventory report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={scope}

Delimitations:
- Focus on naming: files, directories, identifiers, variables
- Do not assess naming quality

Notes:
- Identify case conventions (camelCase, snake_case, kebab-case)
- Note prefixes, suffixes, and delimiters
- Catalog abbreviation patterns
```
