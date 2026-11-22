# Multi-Source Research

Use when research benefits from parallel agents specializing in different source types. Launch one agent per source category to maximize coverage and minimize per-agent scope.

Launch parallel research agents for different source types:

Brief template for a sub-agent:

```text
Research {topic} from web sources and documentation.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `research_procedure` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-research-web" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_QUESTIONS={questions}

Delimitations:
- Web and documentation sources only
```

Brief template for a sub-agent:

```text
Research {topic} from academic sources.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `research_procedure` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-research-academic" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_QUESTIONS={questions}

Delimitations:
- Academic papers and formal specifications only
```
