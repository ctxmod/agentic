---
description: "Technical debt analysis phase — parallel discovery per category, impact assessment, prioritization, roadmap planning."
binds_to_depth: phase
---

# Analyze Debt Phase

Technical debt analysis phase — inventory technical debt across
categories, assess impact, prioritize, and produce a remediation
roadmap. This is one of five sibling phase skills in the analysis
capability (compare, debt, gap, quality, comprehensive); they share
the same objective (`analysis_objective`) and procedure
(`analysis_procedure`) layers and differ only in the reference
material they dispatch against.

Debt analysis is a **phase, not a workflow**. The main agent invokes
it directly via the absorbed `/analyze_debt` slash-command, or any
other domain's workflow agent references this phase as an "inventory
technical debt" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Inventorying technical debt across categories
- Impact assessment guides remediation priority
- Remediation requires phased roadmap
- Debt reduction aligns with strategic objectives

Debt analysis applies to any project with accumulated maintenance
burden: architecture issues, dependency problems, convention
violations, documentation gaps.

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a four-step
sequence. The same objective (`analysis_objective`) runs each time;
the phase-specific reference material below scopes each dispatch.

```
Phase 1: Discovery (Parallel)
         ├── Architecture debt
         ├── Dependencies debt
         ├── Conventions debt
         └── Documentation debt
         ↓
Phase 2: Impact Assessment
         ↓
Phase 3: Prioritization
         ↓
Phase 4: Roadmap Planning
```

### Phase 1: Discovery (Parallel)

Launch one discovery sub-agent per debt category in a single message.
Standard categories: architecture, dependencies, conventions,
documentation. Each agent focuses on single-category debt indicators;
catalog debt items with locations and relationships.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analyze_debt_phase/reference/debt.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery-{category}.md` (parallel)

### Phase 2: Impact Assessment

Sequential pattern aggregating all category discoveries. Evaluate
maintenance burden per item; assess risk exposure and compound
effects; identify debt dependencies.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_general.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-impact-assessment.md`

### Phase 3: Prioritization

Sequential pattern following impact assessment. Apply cost-of-delay
analysis; consider remediation effort; group related items for batch
resolution.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_prioritization.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-debt-prioritization.md`

### Phase 4: Roadmap Planning

Sequential pattern following prioritization. Sequence remediation
phases; define milestones and checkpoints; align with strategic
objectives; recommend integration with feature work.

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/general.md`

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-debt-roadmap.md`

## Information Flow

```
Phase 1: Discovery (Parallel)
- Receives: {inherited work item context}, debt categories
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery-{category}.md (parallel)

Phase 2: Impact Assessment
- Receives: {inherited}, Phase 1 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-impact-assessment.md

Phase 3: Prioritization
- Receives: {inherited}, Phase 2 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-debt-prioritization.md

Phase 4: Roadmap Planning
- Receives: {inherited}, Phase 3 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-debt-roadmap.md
```

Main agent passes report **paths** between phases, never report
content. Each phase reads prior phase report paths as INPUT_REPORTs.

## Variants

**Focused Category Analysis**: Single debt category:
`Discovery → Assessment → Prioritization → Planning`.

**Quick Debt Inventory**: Skip roadmap: `Parallel Discovery → Impact
Assessment → Prioritization`.

**Strategic Debt Planning**: Full workflow with detailed roadmap phase.

## Error Handling

| Situation | Response |
|-----------|----------|
| No debt found in category | Skip category, note in assessment |
| Debt items too granular | Aggregate into larger remediation units |
| Impact assessment unclear | Flag for manual evaluation in prioritization |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Mixing debt discovery with impact assessment | Keep discovery and assessment separate |
| Prioritizing without effort estimates | Include effort in prioritization criteria |
| Roadmap without strategic alignment | Connect remediation to objectives |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`analysis_objective` agent. The objective in turn launches
`analysis_procedure` for step mechanics.

Brief template for the `analysis_objective` agent — Execute <phase-name> within debt analysis:

```text
<ctxmod_activate skill="agentic_workflows_analysis_objective" />
Variant: synthesis
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (discovery) | dispatched via discovery_objective | broad-scan, pattern-detection |
| Phase 2 (impact) | synthesis | impact-scoring, confidence-assessment |
| Phase 3 (prioritization) | synthesis | prioritization |
| Phase 4 (roadmap) | dispatched via planning_objective | (planning procedures) |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"inventory technical debt" as a sub-step. Launch the
`analyze_debt_phase` agent from any workflow that needs debt
assessment.

Brief template for the `analyze_debt_phase` agent — Execute technical debt analysis phase:

```text
<ctxmod_activate skill="agentic_workflows_analyze_debt_phase" />
Execute the full discovery → impact → prioritization → roadmap chain.
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
