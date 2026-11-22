# Academic Discovery

Use when mapping the academic literature around a topic before deep targeted research. Identifies key authors, seminal papers, active debates, research gaps, and methodological approaches through broad scanning.

`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`

Brief template for a sub-agent:

```text
Scan academic literature on {topic} to map the research terrain.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::research@scholarly_prose/reference/broad_scan.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/query_formulation_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/source_selection_academic.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- TOPIC={research topic}
- SCOPE={breadth and boundaries of the scan}

Delimitations:
- Academic sources only (journals, conferences, preprints)
- Map the terrain; do not perform deep analysis of individual papers
- Do not modify any files
- Cite all sources with proper academic citations

Notes:
- Decompose the topic into 3-7 independent sub-queries per the query-formulation procedure
- Execute sub-queries in parallel using academia_search
- Identify seminal papers by citation count and cross-reference frequency
- Record key authors and research groups appearing across multiple results
- Note areas of consensus, active debate, and gaps where no sources exist
- Stop scanning when new queries return familiar results (saturation signal)
- Output a prioritized list of areas warranting deeper investigation
```
