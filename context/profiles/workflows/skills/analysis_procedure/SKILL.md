---
description: "Step-level analysis mechanics — report ingestion, pattern aggregation, contradiction resolution, confidence assessment, criteria weighting, impact scoring, prioritization, and academic critical analysis."
binds_to_depth: procedure
---

# Analysis Procedures

Execute a single, bounded analysis procedure and return its result. Each
procedure below is a self-contained set of prerequisites, steps, and
decision points; the variant material lives in this skill's `reference/`
directory. Where flat (general) and academic variants exist, read only the
variant matching the assigned source type.

## Procedure Selection

Read the reference file matching the assigned task and source type. Do not
read all variants — each is independently applicable and reading extras
wastes context.

| Assigned task | Read (general) | Read (academic) |
|---------------|----------------|-----------------|
| Ingest findings from input reports | `reference/report_ingestion.md` | — |
| Identify themes across multiple sources | `reference/pattern_aggregation.md` | `reference/pattern_aggregation_academic.md` |
| Resolve conflicts between sources | `reference/contradiction_resolution.md` | `reference/contradiction_resolution_academic.md` |
| Assess confidence levels for findings | `reference/confidence_assessment.md` | `reference/confidence_assessment_academic.md` |
| Weight evaluation criteria | `reference/criteria_weighting.md` | — |
| Score impact by business value, risk, frequency | `reference/impact_scoring.md` | — |
| Apply impact/effort matrix for prioritization | `reference/prioritization.md` | — |
| Assess individual sources through structured frameworks | — | `reference/critical_analysis.md` |

### Report Ingestion

Extract findings with source attribution from input reports. Create atomic
finding units with clear provenance linking each finding back to its source
report and location.

For general report ingestion, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure/reference/report_ingestion.md" />

### Pattern Aggregation

Identify themes across multiple sources. Aggregate individual findings into
higher-level patterns that reveal cross-source insights.

For general pattern aggregation, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure/reference/pattern_aggregation.md" />

For academic pattern aggregation with formal synthesis methodology, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure/reference/pattern_aggregation_academic.md" />

### Contradiction Resolution

Resolve conflicts between sources with explicit rationale. Document why one
source is favored over another and under what conditions the resolution
would change.

For general contradiction resolution, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure/reference/contradiction_resolution.md" />

For academic contradiction resolution with formal conflict-resolution
frameworks, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure/reference/contradiction_resolution_academic.md" />

### Confidence Assessment

Assess confidence levels for synthesized findings. Every claim carries a
confidence level reflecting evidence quality, source reliability, and
replication status.

For general confidence assessment, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure/reference/confidence_assessment.md" />

For academic confidence assessment using GRADE methodology with
evidence-quality downgrading and upgrading factors, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure/reference/confidence_assessment_academic.md" />

### Criteria Weighting

Weight evaluation criteria for comparative analysis. Ensure criteria are
measurable, weighted, and include both functional and non-functional
dimensions.

For criteria weighting, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure/reference/criteria_weighting.md" />

### Impact Scoring

Score impact by business value, risk, and frequency. Prioritize findings
using structured scoring that surfaces the most actionable items.

For impact scoring, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure/reference/impact_scoring.md" />

### Prioritization

Apply impact/effort matrix for recommendations. Group related items for
batch resolution and sequence by dependency and impact.

For prioritization, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure/reference/prioritization.md" />

### Critical Analysis

Academic-only. Assess individual sources through structured analytical
frameworks covering methodology, theoretical framework, contribution, and
dialogic positioning. Prevents impressionistic judgments and produces
reusable evaluations that feed into cross-source synthesis.

For academic critical analysis, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure/reference/critical_analysis.md" />

## Execution Discipline

- Read only the variant matching the assigned source type; do not read
  all variants.
- Execute only the assigned procedure; do not expand into adjacent steps.
- Report results to the caller-provided path; return a summary, not the
  full body.
- Reference findings with `file:line` ranges rather than inlining content.
- Note explicitly when a procedure's prerequisites are unmet rather than
  improvising a substitute.

## Parent-Layer Dispatch

This procedure is dispatched by the shared `analysis_objective`,
which in turn is dispatched by five sibling phase skills in the
analysis capability:

| Phase skill | Procedures typically dispatched |
|-------------|---------------------------------|
| `analyze_compare_phase` | criteria-weighting, impact-scoring, confidence-assessment, pattern-aggregation, contradiction-resolution |
| `analyze_debt_phase` | impact-scoring, prioritization, pattern-aggregation |
| `analyze_gap_phase` | criteria-weighting, pattern-aggregation, contradiction-resolution, prioritization, impact-scoring |
| `analyze_quality_phase` | criteria-weighting, impact-scoring, confidence-assessment, pattern-aggregation, contradiction-resolution |
| `analyze_comprehensive_phase` | criteria-weighting, impact-scoring, contradiction-resolution, pattern-aggregation, confidence-assessment, prioritization |

Ad-hoc procedure-depth agents may also load this skill for general
analysis step mechanics outside any of the five capability phase
chains.

- the /analyze command selection guidance — which analysis variant fits
  the assigned analytical question — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/analyze.md" />

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
