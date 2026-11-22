# Specifications Research

Use for researching from standards bodies, RFCs, and formal specifications. Runs in parallel with other academic source searches.

Brief template for a sub-agent:

```text
Research {topic} from formal specifications and standards documents.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `scholarly_research_phase` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{query-formulation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-research-specifications" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_QUESTIONS={queries from Phase 1}

Delimitations:
- Standards bodies, RFCs, formal specifications only
- Do not modify any files
- Cite all sources with version numbers

Notes:
- Use perplexity_search targeting standards organizations
- Identify relevant RFCs, ISO standards, or formal specs
- Note normative vs informative sections
- Extract formal definitions and requirements
```
