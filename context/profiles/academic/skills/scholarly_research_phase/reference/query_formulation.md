# Academic Query Formulation Phase

Use for formulating effective academic search queries from a research
topic. First phase in the academic research workflow; runs sequentially
before parallel academic source search.

Brief template for a sub-agent:

```text
Formulate academic search queries for researching: {topic}

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `scholarly_research_phase` skill
- `ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/query_formulation_academic.md`
- `ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Query formulation only; do not execute searches
- Generate 3-5 distinct query variants for academic databases
- Include formal terminology and research-area keywords

Notes:
- Target academic databases: arXiv, Semantic Scholar, DBLP, Crossref, OpenAlex
- Include publication-venue and year filters
- Identify potential authoritative sources to prioritize
```
