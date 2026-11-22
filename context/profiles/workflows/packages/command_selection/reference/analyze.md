# Analyze Command Selection Guidance

Selection guidance for the `/analyze` command — the unified entry
point for comprehensive multi-dimensional analysis. This document
captures variant-selection logic, comparison matrices, and resource
cost guidance. Execution lives in the analysis phase skills
(`analyze_gap_phase`, `analyze_compare_phase`, `analyze_quality_phase`,
`analyze_debt_phase`, `analyze_comprehensive_phase`) and the
`analysis_objective`/`analysis_procedure` layers.

## Variant Comparison

`/analyze` dispatches to the comprehensive workflow. Use specialized
commands for single-dimension analysis.

| Command | Scope | When to Use |
|---------|-------|-------------|
| `/analyze` | All dimensions | Major decisions, comprehensive assessment |
| `/analyze_gap` | Requirements vs current | Progress validation, scope definition |
| `/analyze_compare` | Alternative evaluation | Technology selection, approach decisions |
| `/analyze_quality` | Quality dimensions | Pre-release assessment, maintenance planning |
| `/analyze_debt` | Technical debt | Debt inventory, remediation roadmapping |

## Work Item Context

Reports nest within work item if context provided. Comprehensive
analysis supports significant decisions requiring multi-dimensional
assessment before commitment — architectural changes, major
refactoring, strategic planning.

## Workflow Shape

The comprehensive analysis workflow (`analysis-workflow` agent)
executes five phases:

1. **Scope Resolution**: Determine applicable analysis dimensions
2. **Parallel Discovery**: Gather information for all dimensions
   (current state, target state, alternatives, quality, debt)
3. **Parallel Assessment**: Assess each dimension based on discovery
   findings
4. **Aggregation**: Aggregate findings across dimensions by theme
5. **Synthesis**: Comprehensive synthesis with recommendations (Opus
   for complex reasoning)

For the per-phase agent templates and information flow, launch the
`analysis-workflow` agent — the workflow skill is preloaded.

## Resource Cost Notes

Comprehensive analysis runs **5 parallel discovery agents and 4
parallel assessment agents**. This provides broad coverage but
consumes more resources than specialized commands. Use for decisions
where incomplete assessment carries high risk.

The aggregation phase (Phase 4) groups findings thematically before
synthesis, reducing the cognitive load on the synthesis agent and
improving coherence of the final report.

## Decision Guide

| Situation | Recommended Command |
|-----------|---------------------|
| Major architectural decision requiring full assessment | `/analyze` |
| Validate progress against defined target | `/analyze_gap` |
| Choose between alternative technologies/approaches | `/analyze_compare` |
| Pre-release quality gate | `/analyze_quality` |
| Inventory debt and plan remediation | `/analyze_debt` |
| Focused single-dimension analysis | Specialized command, not `/analyze` |
| Analysis requiring external evidence | Combine `/research` commands first, then analyze |

## Cross-Capability Guidance

For analysis requiring external research, combine with `/research`
commands to gather evidence before analysis. The `/research` output
reports feed into `/analyze` discovery phases as prior-art input.

For very large scopes, consider decomposing into artifact categories
and running multiple specialized analysis commands before a unified
synthesis.

## Execution Pointers

- Comprehensive analysis → launch `analysis-workflow` (workflow agent)
- Per-dimension analysis → launch the matching phase skill
  (`analyze_gap_phase`, `analyze_compare_phase`,
  `analyze_quality_phase`, `analyze_debt_phase`)
- All variants share `analysis_objective` and `analysis_procedure`
  layers — they differ only in the reference material they dispatch
  against

See the `analyze_comprehensive_phase` skill
and `ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`.
