# Academic Research

Use for comprehensive academic research following the full pipeline: query formulation, parallel source searching, evidence extraction, source evaluation, and synthesis. Covers all academic source types with systematic rigor.

Unlike narrow source-type phases (academic-papers, surveys), this phase executes the complete academic research pipeline as a single agent responsibility.

Brief template for a sub-agent:

```text
Research {topic} through the full academic research pipeline.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarship@academic/reference/research_objective_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/pipeline.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/query_formulation_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/source_evaluation_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/evidence_extraction_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/source_selection_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/bias_detection_academic.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-research-academic" --owner <work item, if any>` immediately before writing; returns the path
- RESEARCH_QUESTIONS={specific questions to answer}

Delimitations:
- Academic sources only: peer-reviewed papers, preprints, surveys, conference proceedings, standards
- Follow all three pipeline phases: Discovery, Analysis, Synthesis
- Do not modify any files
- Cite all sources with proper academic citations

Notes:
- Decompose research questions into 3-7 sub-queries before searching
- Use academia_search across multiple databases for broad coverage
- Apply CRAAP and SIFT frameworks for source evaluation
- Extract evidence atomically: one finding per unit with source location
- Assign confidence levels to each extracted finding
- Flag bias indicators and methodological limitations
- Cluster findings thematically rather than summarizing source by source
- Document contradictions with both positions and their evidence bases
- Catalogue research gaps with implications
```
