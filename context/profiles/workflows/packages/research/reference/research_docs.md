# Documentation Research

Phase for researching documentation best practices, patterns, and external examples.

Use when researching documentation best practices, organizational patterns, or external examples to inform documentation creation or restructuring decisions.

Brief template for a sub-agent:

```text
Research {documentation topic} to inform {documentation decision}.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `research_procedure` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Read if relevant:
- `ctx:github.com/ctxmod/agentic/context::shared::research_procedure@workflows/reference/query_formulation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::research_procedure@workflows/reference/source_selection.md`
- `ctx:github.com/ctxmod/agentic/context::shared::research_procedure@workflows/reference/source_evaluation.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-research-docs" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_TOPIC={specific documentation topic}
- SOURCES={web|academic|standards|all}

Delimitations:
- Research external sources only; do not examine internal codebase
- Evaluate source quality before inclusion
- Document findings with full attribution
- Do not modify any files

Notes:
- Use perplexity_search for web sources on documentation practices
- Use academia_search for research on information architecture and technical communication
- Source evaluation prioritizes Tier 1 (authoritative standards bodies) and Tier 2 (established publications)
- Documentation research follows standard research methodology
```

## Related

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_general.md" />
- `ctx:github.com/ctxmod/agentic/context::shared::writing_phase@workflows/reference/docs.md`
- `ctx:github.com/ctxmod/writing/context::shared::procedures@documentation/PACKAGE.md`
