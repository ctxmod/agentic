# Surveys Research

Use for researching from survey papers, literature reviews, and systematic reviews. Runs in parallel with other academic source searches.

Brief template for a sub-agent:

```text
Research {topic} from scholarly surveys and review papers.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `scholarly_research_phase` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{query-formulation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-research-surveys" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_QUESTIONS={queries from Phase 1}

Delimitations:
- Survey papers, literature reviews, systematic reviews only
- Do not modify any files
- Cite all sources with proper academic citations

Notes:
- Use academia_search with survey/review filters
- Surveys provide comprehensive field overviews
- Extract taxonomies and classification schemes
- Note research gaps identified in surveys
```
