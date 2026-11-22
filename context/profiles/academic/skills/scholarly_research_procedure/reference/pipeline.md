# Research Pipeline

Execute multi-phase academic research from discovery through synthesis

Academic research proceeds through three phases: discovery, analysis, and synthesis. Each phase has distinct activities, outputs, and quality gates that determine readiness for the next phase.

## Phase Overview

| Phase | Activities | Outputs | Quality Gate |
|-------|-----------|---------|--------------|
| Discovery | Query formulation, source identification, initial screening | Source collection, relevance judgments | Sufficient coverage across research questions |
| Analysis | Full-text review, evidence extraction, quality assessment | Structured evidence, quality ratings | Extraction completeness, confidence levels assigned |
| Synthesis | Thematic clustering, gap analysis, integration | Synthesized findings, identified gaps | Coherent narrative, contradictions resolved or documented |

## Discovery Phase

Discovery establishes the evidence base. Breadth matters more than depth at this stage.

1. **Decompose research questions** into 3-7 independent sub-queries, each targeting a distinct dimension of the topic
2. **Execute queries in parallel** across source types: academic databases for peer-reviewed work, grey literature for practice-oriented evidence, and reference chains from high-quality seed papers
3. **Screen results** by title and abstract for relevance against predefined inclusion criteria
4. **Record relevance judgments** with brief justifications for each included and excluded source
5. **Assess coverage** by mapping included sources against research questions to identify under-represented areas

The phase completes when every research question has sources from at least two independent search paths. If coverage gaps remain, refine queries and repeat steps 2-4 for the gap areas.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/query_formulation_academic.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/source_selection_academic.md" />

## Analysis Phase

Analysis converts raw sources into structured, comparable evidence units.

1. **Review full text** with structured reading, focusing on methodology, findings, and stated limitations
2. **Extract evidence** into atomic units: one finding, claim, or data point per unit, each tagged with source, page, and extraction date
3. **Assess source quality** using evaluation frameworks appropriate to the discipline and source type
4. **Assign confidence levels** to each extracted finding based on methodological rigor, sample characteristics, and replication status
5. **Flag bias indicators** and methodological limitations that qualify the strength of extracted evidence

The phase completes when all included sources have structured extractions and confidence ratings. Incomplete extractions or missing confidence levels indicate remaining work.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/evidence_extraction_academic.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/source_evaluation_academic.md" />

## Synthesis Phase

Synthesis integrates individual evidence units into collective understanding.

1. **Cluster findings thematically** across sources rather than summarizing source by source
2. **Identify consensus** where multiple independent sources converge on compatible findings
3. **Document disagreement** with both positions, their evidence bases, and possible explanations for the conflict
4. **Perform gap analysis** to surface topics, methodologies, or contexts absent from the evidence base
5. **Produce integrated findings** with evidence chains linking claims back through extractions to original sources

The phase completes when findings form a coherent narrative, contradictions are either resolved or explicitly documented, and gaps are catalogued with their implications.

`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation_academic.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/confidence_assessment_academic.md`

## Progress Tracking

Monitor pipeline throughput at each transition:

- **Sources identified**: total results from discovery queries
- **Sources screened**: title/abstract review completed
- **Sources included**: passed relevance criteria
- **Sources extracted**: full-text review and evidence extraction completed
- **Sources synthesized**: incorporated into thematic analysis

Track research debt separately: unread sources, incomplete extractions, and unsynthesized themes. Diminishing new findings per additional source signals adequate coverage and readiness to close the discovery phase.

## Anti-Patterns

| Anti-Pattern | Consequence | Alternative |
|--------------|-------------|-------------|
| Skipping screening | Analysis time spent on irrelevant sources | Screen by title and abstract before full-text review |
| Source-by-source summarization | Sequential summaries without analytical integration | Cluster findings thematically across sources |
| Premature synthesis | Conclusions drawn from incomplete evidence base | Complete extraction and confidence rating before synthesizing |
| Single-pass discovery | Misses sources outside initial query framing | Iterate queries based on initial results and identified gaps |

`ctx:github.com/ctxmod/writing_academic/context::shared::research@scholarly_prose/reference/broad_scan.md`
