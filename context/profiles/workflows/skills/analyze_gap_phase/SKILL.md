---
description: "Gap analysis phase — discovery (conditional), target resolution, gap comparison, prioritization."
binds_to_depth: phase
---

# Analyze Gap Phase

Gap analysis phase — identify gaps between current state and target
requirements, compare current to target systematically, and
prioritize gaps for remediation planning. This is one of five sibling
phase skills in the analysis capability (compare, debt, gap, quality,
comprehensive); they share the same objective (`analysis_objective`)
and procedure (`analysis_procedure`) layers and differ only in the
reference material they dispatch against.

Gap analysis is a **phase, not a workflow**. The main agent invokes
it directly via the absorbed `/analyze_gap` slash-command, or any
other domain's workflow agent references this phase as a "identify
gaps" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Comparing current state against target specification
- Identifying missing elements relative to requirements
- Validating progress toward acceptance criteria
- Prioritizing gaps for remediation planning

Gap analysis serves planning phases. The output informs
implementation scope and sequencing.

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a four-step
sequence with a conditional first phase. The same objective
(`analysis_objective`) runs each time; the phase-specific reference
material below scopes each dispatch.

```
Phase 1: Discovery (conditional)
         ↓
Phase 2: Target Resolution
         ↓
Phase 3: Gap Analysis
         ↓
Phase 4: Prioritization
```

### Phase 1: Discovery (conditional)

Conditional: skip when an input report is provided via `--input`.
Sequential pattern for single-scope discovery. Focus on elements
comparable to target requirements; catalog existing elements with
locations.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_phase_selection.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery.md` (conditional)

### Phase 2: Target Resolution

Sequential pattern following discovery or using provided input.
Structure target requirements for gap comparison; extract measurable
criteria from specifications; organize requirements by category.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_general.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-target-resolution.md`

### Phase 3: Gap Analysis

Sequential pattern consuming discovery and target reports. Compare
current to target systematically; categorize gaps: fully met,
partially met, not met; quantify gap magnitude where possible.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analyze_gap_phase/reference/gap.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-gap-analysis.md`

### Phase 4: Prioritization

Sequential pattern following gap analysis. Apply impact/effort matrix
to gaps; consider dependencies between gaps; recommend remediation
sequence.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_prioritization.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-prioritization.md`

## Information Flow

```
Phase 1: Discovery (conditional)
- Receives: {inherited work item context}, scope
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery.md (conditional)

Phase 2: Target Resolution
- Receives: {inherited}, Phase 1 output OR --input report
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-target-resolution.md

Phase 3: Gap Analysis
- Receives: {inherited}, Phase 2 output (and Phase 1 if present)
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-gap-analysis.md

Phase 4: Prioritization
- Receives: {inherited}, Phase 3 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-prioritization.md
```

Main agent passes report **paths** between phases, never report
content. Each phase reads prior phase report paths as INPUT_REPORTs.

## Variants

**Direct Gap Analysis**: Skip discovery when input report provided:
`Target Resolution → Gap Analysis → Prioritization`.

**Requirements-Only Analysis**: When target derives from work item
acceptance criteria: `Discovery → Gap Analysis → Prioritization`.

## Error Handling

| Situation | Response |
|-----------|----------|
| Target specification not found | Halt, request target via `--target` parameter |
| Input report invalid | Halt, report path resolution failure |
| Scope too broad | Recommend decomposition into smaller analyses |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Proposing solutions during gap analysis | Limit to gap identification; planning phase handles solutions |
| Skipping prioritization | Always prioritize to guide planning |
| Comparing against implicit targets | Make target specification explicit |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`analysis_objective` agent. The objective in turn launches
`analysis_procedure` for step mechanics.

Brief template for the `analysis_objective` agent — Execute <phase-name> within gap analysis:

```text
<ctxmod_activate skill="agentic_workflows_analysis_objective" />
Variant: gap
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (discovery, conditional) | dispatched via discovery_objective | broad-scan |
| Phase 2 (target) | gap | criteria-weighting |
| Phase 3 (gap) | gap | pattern-aggregation, contradiction-resolution |
| Phase 4 (prioritization) | synthesis | prioritization, impact-scoring |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"identify gaps" as a sub-step. Launch the `analyze_gap_phase` agent
from any workflow that needs gap assessment.

Brief template for the `analyze_gap_phase` agent — Execute gap analysis phase:

```text
<ctxmod_activate skill="agentic_workflows_analyze_gap_phase" />
Execute the full discovery → target → gap → prioritization chain.
```

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every analysis phase
variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_phase_selection.md" />

For a single-scope analysis that does not need the full four-phase
chain, dispatch the general phase template directly:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_general.md" />

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_analysis.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped analysis guidance for the sub-agents this phase launches:

the `analysis_objective` skill
the `analysis_procedure` skill
