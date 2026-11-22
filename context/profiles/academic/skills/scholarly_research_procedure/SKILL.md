---
description: "Step-level academic research mechanics — academic query formulation, scholarly source selection, academic evidence extraction with formal citation, peer-review source evaluation, academic publication bias detection, broad landscape scanning, and academic pipeline quality-gate enforcement."
binds_to_depth: procedure
---

# Research Academic Procedures

Execute a single, bounded academic research procedure and return its
result. Each procedure below is a self-contained set of prerequisites,
steps, and decision points; the academic-variant material lives in
this skill's `reference/` directory.

This skill carries **only academic mechanics** — scholarly source
selection across peer-reviewed databases, formal citation standards,
methodology evaluation, academic publication-bias detection. General
(web/community/documentation) mechanics live in the sibling
`research_procedure` skill. The two procedure skills share no
methodology: academic source evaluation involves peer review,
citation analysis, and study-design assessment; general involves
web-source trust signals and recency heuristics.

## Procedure Selection

Read the reference file matching the assigned task. Do not read all
procedures — each is independently applicable and reading extras
wastes context.

| Assigned task | Read |
|---------------|------|
| Construct academic sub-queries across scholarly databases | `reference/query_formulation_academic.md` |
| Select academic sources matching question types | `reference/source_selection_academic.md` |
| Extract atomic findings with formal academic citations | `reference/evidence_extraction_academic.md` |
| Assess academic source quality (study design, methodology) | `reference/source_evaluation_academic.md` |
| Detect academic publication and methodology bias | `reference/bias_detection_academic.md` |
| Scan broad landscape of a topic across scholarly databases | `reference/broad_scan.md` |
| Enforce academic pipeline quality gates between phases | `reference/pipeline.md` |

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/bias_detection_academic.md" />
- `ctx:github.com/ctxmod/writing_academic/context::shared::research@scholarly_prose/reference/broad_scan.md` (the scholarly broad-scan method, kept in the writing_academic module)
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/evidence_extraction_academic.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/pipeline.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/query_formulation_academic.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/source_evaluation_academic.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/source_selection_academic.md" />

### Academic Query Formulation

Constructing academic search queries requires sub-query decomposition
across scholarly databases (arXiv, Semantic Scholar, DBLP, Crossref,
OpenAlex). Queries target formal terminology and research-area
keywords; include publication-venue and year filters.

For academic query formulation, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/query_formulation_academic.md" />

### Academic Source Selection

Match academic question types to scholarly databases based on source
characteristics. arXiv for preprints, Semantic Scholar for citation
networks, DBLP for computer science bibliography, Crossref for
cross-discipline coverage, OpenAlex for open-access breadth.

For academic source selection, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/source_selection_academic.md" />

### Academic Evidence Extraction

Create atomic findings with formal academic citations (author, year,
DOI). Avoid direct content copying; paraphrase with attribution. Each
evidence unit represents a single finding with provenance linking back
to source location.

For academic evidence extraction, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/evidence_extraction_academic.md" />

### Academic Source Evaluation

Assess academic source quality using study design, sample
characteristics, methodology rigor, and venue reputation. Apply
frameworks (CRAAP, SIFT adapted for scholarly work) and assign
confidence levels grounded in evidence strength.

For academic source evaluation, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/source_evaluation_academic.md" />

### Academic Bias Detection

Identify publication bias, funding-source conflicts, methodological
limitations, and other indicators that qualify academic evidence
strength. Bias detection is systematic: check declarations, assess
methodology, compare across studies, trace citation networks.

For academic bias detection, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/bias_detection_academic.md" />

### Broad Scan

Scan the broad landscape of a topic across scholarly databases to
establish coverage before deeper analysis. Decompose research
questions into sub-queries targeting different facets and issue the
database queries concurrently. These are concurrent tool calls, not
sub-agent dispatch, so the `allow-parallel-agents` posture does not
apply to them.

`ctx:github.com/ctxmod/writing_academic/context::shared::research@scholarly_prose/reference/broad_scan.md` (the scholarly broad-scan method, kept in the writing_academic module)

### Academic Pipeline

Enforce the academic research pipeline's quality gates between the
discovery, analysis, and synthesis phases. The pipeline defines
minimum source counts, extraction completeness thresholds, and
coverage criteria that must be met before proceeding between phases.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/pipeline.md" />

## Execution Discipline

- Read only the procedure matching the assigned task; do not read all
  variants.
- Execute only the assigned procedure; do not expand into adjacent steps.
- Report results to the caller-provided path; return a summary, not the
  full body.
- Reference findings with `file:line` ranges or DOI/URL rather than
  inlining content.
- Note explicitly when a procedure's prerequisites are unmet rather than
  improvising a substitute.
- All claims require inline citations with complete metadata (authors,
  title, venue, year, DOI).

## Conditional-Link Dispatch Table

| Upstream phase | Procedure to dispatch |
|----------------|-----------------------|
| `scholarly_research_phase` (Phase 1 query formulation) | `reference/query_formulation_academic.md` |
| `scholarly_research_phase` (Phase 2 academic search) | `reference/source_selection_academic.md` + `reference/evidence_extraction_academic.md` |
| `scholarly_research_phase` (Phase 3 synthesis prep) | `reference/source_evaluation_academic.md` + `reference/bias_detection_academic.md` |
| ad-hoc academic pipeline execution | `reference/broad_scan.md` + `reference/pipeline.md` |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_academic/context::shared::citation_practice@scholarly_prose/reference/citation_practice.md`
