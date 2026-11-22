# External Discovery

Use when internal discovery alone is insufficient and external sources must inform the workflow. Focuses on web, academic, and standards sources.

`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`

Brief template for a sub-agent:

```text
Research {topic} to inform {decision or approach}.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- `ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-external" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_QUESTIONS={specific questions to answer}

Delimitations:
- Focus on {source types: web, academic, standards}
- Do not modify any files

Notes:
- Cite all sources
- Note contradictions between sources
```

## Variant-Specific Notes

Use external discovery variant when internal discovery alone is insufficient and external sources must inform the workflow.
