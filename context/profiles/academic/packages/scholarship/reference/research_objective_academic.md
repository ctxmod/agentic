# Academic Research Objective

Conduct systematic academic research using peer-reviewed sources, scholarly databases, and formal citation standards. This variant extends the base research objective with academic methodology: structured literature search across databases, quality assessment of study design and methodology, and synthesis following IMRaD conventions.

## Base Behavior

the `scholarly_research_procedure` skill

## Academic Methodology

Three phases structure the academic research workflow.

### Phase 1: Discovery

Brief template for a procedure-depth agent (launched from objective depth):

```text
Discover academic sources by decomposing research questions into sub-queries and searching scholarly databases in parallel, screening results by title and abstract.
`ctx:github.com/ctxmod/writing_academic/context::shared::research@scholarly_prose/reference/broad_scan.md`
`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/query_formulation_academic.md`
`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/source_selection_academic.md`
```

Decompose research questions into sub-queries targeting different facets. Execute across academic databases in parallel: arXiv for preprints, Semantic Scholar for citation networks, DBLP for computer science bibliography, Crossref for cross-discipline coverage, OpenAlex for open-access breadth. Screen results by title and abstract against inclusion criteria. Assess coverage across research questions and iterate for gaps until sufficient evidence exists.

### Phase 2: Analysis

Brief template for a procedure-depth agent (launched from objective depth):

```text
Analyze selected sources through full-text review: extract atomic evidence with attribution, assess methodological quality, assign confidence, and flag bias.
`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/source_evaluation_academic.md`
`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/evidence_extraction_academic.md`
`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/bias_detection_academic.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/critical_analysis.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/confidence_assessment_academic.md`
```

Full-text review with structured reading. Extract atomic evidence units with source attribution (author, year, DOI). Assess quality using study design, sample characteristics, and methodology rigor. Assign confidence levels based on evidence strength. Flag funding sources, conflicts of interest, and methodological limitations.

### Phase 3: Synthesis

Brief template for a procedure-depth agent (launched from objective depth):

```text
Synthesize findings by clustering evidence thematically across sources, identifying consensus, and resolving contradictions.
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation_academic.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/contradiction_resolution_academic.md`
```

Cluster findings thematically across sources. Identify consensus areas where multiple studies converge. Document disagreements with explanations grounded in methodology differences, sample variation, or temporal context. Perform gap analysis to identify under-researched areas. Produce integrated findings with evidence chains linking claims to specific studies.

## Academic Pipeline

Brief template for a procedure-depth agent (launched from objective depth):

```text
Enforce the academic pipeline's quality gates between the discovery, analysis, and synthesis phases.
`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/pipeline.md`
```

The pipeline defines quality gates between phases: minimum source count before proceeding from discovery to analysis, minimum extraction completeness before synthesis, and coverage thresholds across research questions.

## Citation Requirements

`ctx:github.com/ctxmod/writing_academic/context::shared::citation_practice@scholarly_prose/reference/citation_practice.md`

`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_language.md`

`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_voice.md`

All claims require inline citations with complete metadata. Use the citation style specified in parameters (APA, MLA, Chicago, or IEEE). Prefer narrative citations when discussing a study's contribution ("Smith et al. (2024) demonstrated...") and parenthetical citations for supporting evidence ("...consistent with prior findings (Jones, 2023; Lee, 2024)"). Every cited source appears in the references section with full bibliographic data: authors, title, venue, year, DOI or URL.

## Report Structure

Reports follow IMRaD organization with academic-specific sections:

| Section | Content |
|---------|---------|
| Abstract | 150-250 word summary: background, methods, results, implications |
| Introduction | Research questions, significance, scope |
| Methodology | Databases searched, queries used, inclusion/exclusion criteria, extraction process |
| Results | Thematic findings with evidence chains and confidence levels |
| Discussion | Interpretation, limitations, implications for practice |
| References | Complete bibliography in specified citation style |
| Appendix | Full search queries, database coverage, relevance judgments |

`ctx:github.com/ctxmod/writing_academic/context::shared::composition@scholarly_prose/reference/limitations.md`

`ctx:github.com/ctxmod/writing_academic/context::shared::process@scholarly_prose/reference/revision.md`

`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_register.md`

`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/terminology.md`

`ctx:github.com/ctxmod/writing_academic/context::shared::composition@scholarly_prose/reference/depth_placement.md`

`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/concreteness.md`

`ctx:github.com/ctxmod/writing_academic/context::shared::composition@scholarly_prose/reference/exhibits.md`

`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/rhythm.md`

`ctx:github.com/ctxmod/writing_academic/context::shared::research@scholarly_prose/reference/broad_scan.md`

## Tool Permissions

**Allowed**: `academia_search` (`mcp__academia__academia_search`), `perplexity_search`, `WebSearch`, `WebFetch`, Read, Write (reserved report path only)

**Forbidden**: Modifying source files, making implementation decisions, launching sub-agents

The `academia_search` and `perplexity_search` tool are the primary search instruments. Use `perplexity_search` for supplementary context and grey literature comparison, or pre-scanning for wider search before performing focused search using `academia_search`. Use `WebFetch` for accessing specific paper URLs or institutional pages.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Grey literature as primary evidence | Lower methodological rigor | Prioritize peer-reviewed sources; use grey literature as supplementary |
| Missing methodological assessment | Unknown evidence quality | Evaluate study design, sample, and analysis method |
| Ignoring publication venue | Quality signals vary across venues | Assess journal impact, review process, publisher standing |
| Incomplete citations | Claims become unverifiable | Full metadata per citation style: authors, title, venue, year, DOI |
| Premature synthesis | Incomplete evidence base | Complete extraction across all research questions before synthesizing |
| Undocumented limitations | Overconfident claims | State constraints from study design, sample scope, and temporal bounds |

## Examples

**Systematic Review**: Query "transformer architectures for long document summarization" via `academia_search` with `year_min: 2022`. Scan 40+ papers across arXiv and Semantic Scholar. Extract methodology comparisons, benchmark results, architectural innovations. Synthesize into thematic clusters: attention mechanisms, chunking strategies, hierarchical approaches. Report: IMRaD structure with 25 cited sources, GRADE confidence levels, identified gap in cross-lingual evaluation.

**Targeted Investigation**: Query "reinforcement learning reward shaping safety constraints" via `academia_search`. Focus on 8-12 high-citation papers from top venues (NeurIPS, ICML, ICLR). Extract reward formulation patterns and safety guarantee proofs. Report: focused analysis with methodology comparison table, 3 consensus findings at High confidence, 2 at Moderate.

**Cross-Disciplinary Survey**: Query "graph neural networks for molecular property prediction drug discovery" via `academia_search` across arXiv and Semantic Scholar. Bridge ML methodology papers with pharmaceutical application papers. Report: IMRaD with dual-perspective synthesis, citation network analysis, identified gap between theoretical advances and clinical validation.

## Related

`ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`

`ctx:github.com/ctxmod/agentic/context::shared::analysis_objective@workflows/reference/synthesis.md`

`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`
