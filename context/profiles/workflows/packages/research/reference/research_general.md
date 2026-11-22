# General Research

Use when researching a single topic from mixed external sources without requiring parallel source-type specialization.

Brief template for a sub-agent:

```text
Research {topic} to inform {decision or approach}.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `research_procedure` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-research" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_QUESTIONS={specific questions to answer}

Delimitations:
- Focus on {source types: web, academic, standards}
- Do not modify any files

Notes:
- Cite all sources
- Note contradictions between sources
```
