# Query Formulation Research

Use for formulating effective search queries from research topics. First phase in most research workflows.

Brief template for a sub-agent:

```text
Formulate search queries for researching: {topic}

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `research_practices_phase` skill
- `ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Query formulation only; do not execute searches
- Generate 3-5 distinct query variants targeting different aspects

Notes:
- Consider different terminology for the topic
- Target {source types: web, academic, standards}
- Identify potential authoritative sources to prioritize
```
