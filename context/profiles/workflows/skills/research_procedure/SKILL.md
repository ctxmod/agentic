---
description: "Step-level research mechanics — query formulation, source selection, evidence extraction, source evaluation, and bias detection for web, documentation, and community sources."
binds_to_depth: procedure
---

# Research Procedures

Execute a single, bounded research procedure and return its result. Each
procedure below is a self-contained set of prerequisites, steps, and
decision points; the variant material lives in this skill's `reference/`
directory.

This skill carries **only general (web, documentation, community)
mechanics**. Academic mechanics — peer-reviewed source evaluation,
formal citation standards, scholarly methodology assessment — live in
the sibling `scholarly_research_procedure` skill. The two procedure
skills share no methodology: general source evaluation involves
web-source trust signals and recency heuristics; academic involves peer
review, citation analysis, and study-design assessment.

## Procedure Selection

Read the reference file matching the assigned task and source type. Do
not read all variants — each is independently applicable and reading
extras wastes context.

| Assigned task | Read |
|---------------|------|
| Construct search queries | `reference/query_formulation.md` |
| Select sources matching question types | `reference/source_selection.md` |
| Extract atomic findings with attribution | `reference/evidence_extraction.md` |
| Assess source quality and credibility | `reference/source_evaluation.md` |
| Detect bias in sources | `reference/bias_detection.md` |

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_procedure/reference/bias_detection.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_procedure/reference/evidence_extraction.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_procedure/reference/query_formulation.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_procedure/reference/source_evaluation.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_procedure/reference/source_selection.md" />

### Query Formulation

Constructing effective search queries requires specificity, technical
precision, and context. Well-formed queries return relevant results
quickly; poorly formed queries waste research time on irrelevant
information.

For web and general source search, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_procedure/reference/query_formulation.md" />

### Source Selection

Match question types to appropriate search tools based on source
characteristics. Different source types serve different research needs:
web sources for current practices, documentation for authoritative
specs, community sources for consensus and pitfalls.

For web, documentation, and community sources, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_procedure/reference/source_selection.md" />

### Evidence Extraction

Create atomic findings with source attribution, avoiding direct content
copying. Each evidence unit should represent a single finding with clear
provenance linking it back to the source location.

For web evidence extraction, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_procedure/reference/evidence_extraction.md" />

### Source Evaluation

Assess source quality using tier-based frameworks, prioritizing recent
and authoritative sources. Quality assessment considers publisher
reputation, methodology rigor, recency, and potential conflicts of
interest.

For web source credibility assessment, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_procedure/reference/source_evaluation.md" />

### Bias Detection

Identify methodological constraints, commercial affiliations, and other
indicators that qualify evidence strength. Bias detection is systematic:
check declarations, assess methodology, compare across sources, trace
citations, and search for counter-evidence.

For web and vendor bias detection, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_procedure/reference/bias_detection.md" />

## Execution Discipline

- Read only the variant matching the assigned source type; do not read
  all variants.
- Execute only the assigned procedure; do not expand into adjacent steps.
- Report results to the caller-provided path; return a summary, not the
  full body.
- Reference findings with `file:line` ranges rather than inlining content.
- Note explicitly when a procedure's prerequisites are unmet rather than
  improvising a substitute.

## Conditional-Link Dispatch Table

| Upstream phase | Procedure to dispatch |
|----------------|-----------------------|
| `research_compare_phase` (Phase 1 criteria definition) | `reference/query_formulation.md` |
| `research_compare_phase` (Phase 2 alternative research) | `reference/source_selection.md` + `reference/evidence_extraction.md` |
| `research_practices_phase` (Phase 1 query formulation) | `reference/query_formulation.md` |
| `research_practices_phase` (Phase 2 search) | `reference/source_selection.md` + `reference/evidence_extraction.md` |
| `research_practices_phase` (Phase 3 synthesis prep) | `reference/source_evaluation.md` + `reference/bias_detection.md` |

For academic procedures (peer-reviewed sources, formal citation
standards, methodology evaluation), dispatch the sibling
`scholarly_research_procedure` skill instead:

the `scholarly_research_procedure` skill

- the /research command selection guidance — which research variant and
  source categories fit the assigned question — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/research.md" />

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/research_briefs.md" />
