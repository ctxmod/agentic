# General Discovery

Use when discovering context from internal artifacts within a single scope. Suitable for most discovery needs where parallel domain specialization is unnecessary.

`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`

Brief template for a sub-agent — Discover context from internal artifacts within a single scope:

```text
{Brief description of what to discover}

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{project context files as needed}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={what to examine}

Delimitations:
- {scope boundaries}
- Do not modify any files

Notes:
- {focus areas}
- {known patterns to look for}
```
