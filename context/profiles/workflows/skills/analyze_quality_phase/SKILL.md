---
description: "Quality analysis phase — discovery, parallel dimension assessment, synthesis, remediation planning."
binds_to_depth: phase
---

# Analyze Quality Phase

Quality analysis phase — assess artifact quality across multiple
dimensions, synthesize findings, and produce a prioritized remediation
plan. This is one of five sibling phase skills in the analysis
capability (compare, debt, gap, quality, comprehensive); they share
the same objective (`analysis_objective`) and procedure
(`analysis_procedure`) layers and differ only in the reference
material they dispatch against.

Quality analysis is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/analyze_quality`
slash-command, or any other domain's workflow agent references this
phase as an "assess quality" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Artifacts require multi-dimensional quality assessment
- Quality evaluation benefits from parallel dimension analysis
- Improvements need prioritization across dimensions
- Remediation guidance expected as output

Quality analysis applies to any artifact type: documentation,
specifications, configurations, source code, tests.

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a four-step
fan-out fan-in sequence. The same objective (`analysis_objective`)
runs each time; the phase-specific reference material below scopes
each dispatch.

```
Phase 1: Discovery
         ↓
Phase 2: Dimension Assessment (Parallel)
         ├── Dimension A assessment
         ├── Dimension B assessment
         └── Dimension C assessment
         ↓
Phase 3: Synthesis
         ↓
Phase 4: Remediation Planning
```

### Phase 1: Discovery

Sequential pattern for single-scope discovery. Catalog artifacts and
determine applicable quality dimensions; identify artifact types for
dimension selection; note existing quality indicators.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_phase_selection.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery.md`

### Phase 2: Dimension Assessment (Parallel)

Launch one assessment sub-agent per dimension in a single message.
Standard dimensions: structure, consistency, completeness, clarity.
Each agent assesses a single dimension across all artifacts; applies
dimension-specific criteria consistently.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analyze_quality_phase/reference/quality.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-assessment-{dimension}.md` (parallel)

### Phase 3: Synthesis

Sequential pattern aggregating all dimension assessments. Aggregate
scores across dimensions; identify cross-dimension patterns; rank
artifacts by overall quality.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-quality-synthesis.md`

### Phase 4: Remediation Planning

Sequential pattern following synthesis. Group related issues for
batch remediation; sequence by dependency and impact; identify quick
wins vs strategic improvements.

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/general.md`

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-remediation-plan.md`

## Information Flow

```
Phase 1: Discovery
- Receives: {inherited work item context}, scope, dimensions
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery.md

Phase 2: Dimension Assessment (Parallel)
- Receives: {inherited}, Phase 1 output, dimensions list
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-assessment-{dimension}.md (parallel)

Phase 3: Synthesis
- Receives: {inherited}, Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-quality-synthesis.md

Phase 4: Remediation Planning
- Receives: {inherited}, Phase 3 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-remediation-plan.md
```

Main agent passes report **paths** between phases, never report
content. Each phase reads prior phase report paths as INPUT_REPORTs.

## Variants

**Single-Dimension Assessment**: Skip synthesis: `Discovery →
Assessment → Planning`.

**Focused Quality Check**: Skip remediation planning: `Discovery →
Assessment → Synthesis`.

**Comprehensive Quality Audit**: Standard four dimensions plus
domain-specific dimensions.

## Error Handling

| Situation | Response |
|-----------|----------|
| Artifact type not recognized | Default to general quality dimensions |
| Dimension inapplicable to artifact | Skip dimension, note in synthesis |
| Assessment scores inconsistent | Flag for manual review in synthesis |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Assessing quality without criteria | Define dimension-specific criteria explicitly |
| Mixing assessment and remediation | Keep phases separate for clarity |
| Single score for multi-dimensional quality | Provide dimension breakdown in synthesis |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`analysis_objective` agent. The objective in turn launches
`analysis_procedure` for step mechanics.

Brief template for the `analysis_objective` agent — Execute <phase-name> within quality analysis:

```text
<ctxmod_activate skill="analysis_objective" />
Variant: synthesis
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (discovery) | dispatched via discovery_objective | broad-scan |
| Phase 2 (assessment) | synthesis | criteria-weighting, impact-scoring, confidence-assessment |
| Phase 3 (synthesis) | synthesis | pattern-aggregation, contradiction-resolution |
| Phase 4 (remediation) | dispatched via planning_objective | (planning procedures) |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"assess quality" as a sub-step. Launch the `analyze_quality_phase`
agent from any workflow that needs quality assessment.

Brief template for the `analyze_quality_phase` agent — Execute quality analysis phase:

```text
<ctxmod_activate skill="analyze_quality_phase" />
Execute the full discovery → assessment → synthesis → remediation chain.
```

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every analysis phase
variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_phase_selection.md" />

For a single-scope analysis that does not need the full four-phase
chain, dispatch the general phase template directly:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_general.md" />

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_analysis.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped analysis guidance for the sub-agents this phase launches:

the `analysis_objective` skill
the `analysis_procedure` skill
