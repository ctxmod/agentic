# Web Sources Research

Use for researching from web sources and technical documentation. Runs in parallel with other best practices source searches.

Brief template for a sub-agent:

```text
Research {topic} best practices from web sources and technical documentation.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `research_practices_phase` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{query-formulation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-research-web" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_QUESTIONS={queries from Phase 1}

Delimitations:
- Web and documentation sources only
- Do not modify any files
- Cite all sources with URLs

Notes:
- Use perplexity_search for web research
- Prioritize official documentation and reputable technical blogs
- Note publication dates for recency assessment
```
