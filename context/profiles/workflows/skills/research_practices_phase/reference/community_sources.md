# Community Sources Research

Use for researching from developer communities and forums. Runs in parallel with other best practices source searches.

Brief template for a sub-agent:

```text
Research {topic} best practices from developer communities and forums.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `research_practices_phase` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{query-formulation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_QUESTIONS={queries from Phase 1}

Delimitations:
- Community sources: Stack Overflow, Reddit, GitHub discussions
- Do not modify any files
- Cite all sources with URLs

Notes:
- Use perplexity_search targeting community sources
- Note consensus patterns vs minority opinions
- Identify common pitfalls and anti-patterns mentioned
```
