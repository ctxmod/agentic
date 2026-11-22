---
description: "Interpret discovery and research findings to produce decision-ready analysis reports with insights, recommendations, and confidence assessments."
binds_to_depth: objective
---

# Analysis Objective

Interpret information gathered through discovery. Read reports, identify
patterns, assess impact, generate recommendations. Produce a comprehensive
analysis report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Variant Selection

Specialized analysis variants add specific methodology to this base
objective. Read the variant matching the assigned analytical question;
variants are additive, not replacements. Each variant corresponds to
one of the five analysis capability phases that dispatch this
objective.

| Phase dispatching | Variant | Read |
|-------------------|---------|------|
| `analyze_compare_phase` | comparative | `reference/comparative.md` |
| `analyze_gap_phase` | gap | `reference/gap.md` |
| `analyze_comprehensive_phase` (and assessment phases) | synthesis | `reference/synthesis.md` |

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_objective/reference/comparative.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_objective/reference/gap.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_objective/reference/synthesis.md" />

Variants must be read when even somewhat relevant.

- the /analyze command selection guidance — which analysis variant fits
  the assigned analytical question — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/analyze.md" />

## Core Behavior

1. **Read**: Consume discovery reports and reference materials
2. **Interpret**: Extract meaning beyond surface-level findings
3. **Evaluate**: Assess significance, impact, and urgency
4. **Synthesize**: Connect findings into coherent understanding
5. **Recommend**: Generate actionable next steps with prioritization
6. **Report**: Write analysis to the reserved report path

## Relationship to Discovery

Discovery and analysis form a pipeline:

| Phase | Focus | Output |
|-------|-------|--------|
| Discovery | What exists | Catalogs, inventories, maps |
| Analysis | What it means | Insights, recommendations, roadmaps |

Analysis objectives receive discovery reports as input. They transform raw findings into decision-ready information.

## Pre-Analysis

Before analyzing:

1. Read all input reports listed in the prompt
2. Understand the analytical question being asked
3. Identify which analysis pattern applies (comparative, gap, synthesis)
4. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

## Parent-Layer Dispatch

This objective is dispatched by five sibling phase skills in the
analysis capability, one per capability variant:

| Phase skill | Variant typically dispatched | Reference material |
|-------------|------------------------------|--------------------|
| `analyze_compare_phase` | comparative | `skills/analyze-compare-phase/reference/*.md` |
| `analyze_debt_phase` | synthesis | `skills/analyze-debt-phase/reference/*.md` |
| `analyze_gap_phase` | gap | `skills/analyze-gap-phase/reference/*.md` |
| `analyze_quality_phase` | synthesis | `skills/analyze-quality-phase/reference/*.md` |
| `analyze_comprehensive_phase` | synthesis | `skills/analyze-comprehensive-phase/reference/*.md` |

Ad-hoc objective-depth agents may also load this skill for general
analysis work outside any of the five capability phase chains.

## During Analysis

Apply these core procedures as relevant to the analysis type:

- **Report ingestion**: Extract findings with source attribution
  `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/report_ingestion.md`
- **Pattern aggregation**: Identify themes across multiple sources
  `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation.md`
- **Contradiction resolution**: Resolve conflicts with explicit rationale
  `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/contradiction_resolution.md`
- **Confidence assessment**: Assess confidence levels for findings
  `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/confidence_assessment.md`
- **Impact scoring**: Prioritize by business value, risk, frequency
  `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/impact_scoring.md`
- **Prioritization**: Apply impact/effort matrix for recommendations
  `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/prioritization.md`

Variants specify which procedures apply to their analytical question.

## Supporting Procedures

Analysis procedures carry the step-level mechanics. Link conditionally on
what the assigned analytical question requires:

the `analysis_procedure` skill

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create the analysis report by organizing the interpreted findings and writing the structured report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Analysis reports include these objective-specific sections beyond the
standard structure:

- **Findings**: Organized by category with source references
- **Insights**: Interpretations beyond surface-level findings
- **Recommendations**: Actionable next steps with prioritization
- **Confidence**: Assessment of evidence quality per finding
- **Gaps**: Areas requiring further investigation

Target length: 500-2000 words. Longer than discovery reports due to interpretation depth.

## Tool Constraints

**Allowed**:
- Read: Report and reference files
- Grep: Pattern search when verification needed
- Glob: File discovery for supplementary context
- Write: Analysis report at the reserved report path only

**Forbidden**:
- Modifying source files
- Launching sub-agents
- Writing outside the reserved report path
- Making implementation changes

## Response Format

After writing the report, return a brief response:

```
Wrote analysis report to <reserved report path>.

Summary: [2-3 sentence summary of key insights and recommendations]
```

Do not return the full report content. The report file is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Restating discovery findings | No added value | Interpret and assess significance |
| Recommendations without evidence | Ungrounded claims | Cite source reports for each claim |
| Equal weight to all findings | Unclear priorities | Explicitly prioritize by impact |
| Ignoring contradictions | Incomplete analysis | Resolve conflicts with rationale |
| Skipping confidence assessment | Overconfident conclusions | State confidence for uncertain findings |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_before_execution.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

## Sub-Layer Launch

When this objective requires step-level execution, launch the
`analysis_procedure` agent. Procedure agents carry the per-procedure
mechanics under 
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/confidence_assessment_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/confidence_assessment.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/contradiction_resolution_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/contradiction_resolution.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/criteria_weighting.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/critical_analysis.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/impact_scoring.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/prioritization.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/report_ingestion.md`
.

Brief template for the `analysis_procedure` agent — Execute <procedure-name> within analysis objective:

```text
<ctxmod_activate skill="analysis_procedure" />
Procedure: <procedure-name>
<procedure-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Available procedure agents in the analysis domain: dispatch against
the procedure variants in 
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/confidence_assessment_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/confidence_assessment.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/contradiction_resolution_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/contradiction_resolution.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/criteria_weighting.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/critical_analysis.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/impact_scoring.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/prioritization.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/report_ingestion.md`

(report-ingestion, pattern-aggregation, contradiction-resolution,
confidence-assessment, criteria-weighting, impact-scoring,
prioritization, critical-analysis).
