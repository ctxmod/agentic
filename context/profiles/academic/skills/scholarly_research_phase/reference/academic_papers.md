# Academic Papers Research

Use for researching from academic papers and preprints. Runs in parallel with other academic source searches.

Brief template for a sub-agent:

```text
Research {topic} from academic papers and preprints.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarship@academic/reference/research_objective_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{query-formulation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-research-papers" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_QUESTIONS={queries from Phase 1}

Delimitations:
- Academic papers and preprints only
- Do not modify any files
- Cite all sources with proper academic citations

Notes:
- Use academia_search for academic database queries
- Include citation counts as quality signals
- Note seminal papers vs recent advances
- Extract key findings and methodologies
```
