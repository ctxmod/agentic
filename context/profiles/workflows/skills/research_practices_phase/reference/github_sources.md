# GitHub Sources Research

Use for researching from GitHub repositories and code examples. Runs in parallel with other best practices source searches.

Brief template for a sub-agent:

```text
Research {topic} implementations and patterns from GitHub repositories.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `research_practices_phase` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{query-formulation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_QUESTIONS={queries from Phase 1}

Delimitations:
- GitHub repositories and code examples only
- Do not modify any files
- Note repository stars and activity as quality signals

Notes:
- Use perplexity_search with GitHub focus
- Identify well-maintained reference implementations
- Extract patterns from popular repositories
```
