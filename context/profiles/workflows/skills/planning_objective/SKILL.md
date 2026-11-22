---
description: "Translate analysis findings into executable change plans — approach selection, sequencing, dependency mapping, success criteria, and risk mitigation producing structured planning reports."
binds_to_depth: objective
---

# Planning Objective

Translate analysis findings into executable change plans. Planning
objectives read analysis reports, select approaches, sequence steps
with dependencies, define success criteria, and produce a structured
plan at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

Specialized planning patterns exist for different planning scenarios:

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_objective/reference/constraint_based.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_objective/reference/decision_map.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_objective/reference/general.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_objective/reference/incremental.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_objective/reference/phased.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_objective/reference/risk_aware.md" />

Variants must be read when even somewhat relevant.

## Variant Selection

Read the variant matching the assigned task. Each variant adds specific
methodology to this base objective and is additive, not a replacement.

| Planning scenario | Read |
|-------------------|------|
| Single recommended approach, standard execution | `reference/general.md` |
| Multi-phase execution with gates and handoffs | `reference/phased.md` |
| Iterative cycles with adaptation points | `reference/incremental.md` |
| Elevated uncertainty or high-impact failures | `reference/risk_aware.md` |
| Tight resource, time, or capability boundaries | `reference/constraint_based.md` |
| Oversized, decision-fogged effort spanning many sessions | `reference/decision_map.md` |

## Core Behavior

1. **Read**: Consume analysis reports and relevant reference materials
2. **Select**: Choose an approach based on analysis recommendations and constraints
3. **Sequence**: Order steps to respect dependencies and enable parallelization
4. **Define**: Establish success criteria, checkpoints, and rollback triggers
5. **Mitigate**: Identify risks and define contingency responses
6. **Report**: Write plan to the reserved report path
7. **Summarize**: Return brief summary (2-3 sentences) with report path

## Relationship to Discovery, Analysis, and Implementation

Planning bridges analysis and implementation in the workflow:

| Phase | Focus | Output |
|-------|-------|--------|
| Discovery | What exists | Catalogs, inventories, maps |
| Analysis | What to do | Recommendations, priorities, assessments |
| **Planning** | **How to do it** | **Sequenced steps, dependencies, criteria** |
| Implementation | Do it | Modified artifacts |

Planning objectives receive recommendations from analysis. They
transform prioritized findings into step-by-step execution strategies
that implementation agents follow.

## Pre-Planning

Before planning:

1. Read all input analysis reports listed in the prompt
2. Understand the recommended approaches and their trade-offs
3. Identify constraints affecting execution (resource, time, capability, dependency)
4. Assess which planning pattern applies (general, phased, incremental, risk-aware, constraint-based)
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

## During Planning

Apply these core procedures as relevant to the planning scenario:

- **Approach selection**: Evaluate alternatives against criteria and select optimal path
  `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/approach_selection.md`
- **Dependency analysis**: Map execution order and identify critical paths
  `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`
- **Checkpoint definition**: Establish verification points with rollback triggers
  `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/checkpoint_definition.md`
- **Risk assessment**: Evaluate probability and impact, define mitigations
  `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/risk_assessment.md`
- **Constraint mapping**: Define feasible solution space within boundaries
  `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/constraint_mapping.md`

Variants specify which procedures apply to their planning scenario.

## Supporting Procedures

Planning procedures carry the step-level mechanics. Link conditionally
on what the assigned objective requires:

the `planning_procedure` skill

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the planning report path just-in-time and create the structured output following the report organization and creation procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Planning reports include these objective-specific sections beyond the
standard structure:

- **Approach**: Selected strategy with rationale linking to analysis findings
- **Execution Plan**: Sequenced steps with dependencies and responsible parties
- **Success Criteria**: Measurable outcomes for plan completion
- **Risk Mitigation**: Identified risks with probability, impact, and contingency responses

Target length: 500-2000 words. Longer than analysis reports when
multi-phase execution or complex dependencies require detailed
sequencing.

## Tool Constraints

**Allowed**:
- Read: Analysis reports, reference files, related artifacts
- Grep: Search for dependency and constraint information
- Glob: Locate relevant artifacts for scope assessment
- Write: Planning report at the reserved report path only

**Forbidden**:
- Modifying artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Making implementation changes

## Response Format

After writing the report, return a brief response:

```
Wrote planning report to <reserved report path>.

Summary: [2-3 sentence summary of selected approach, key steps, and primary risks]
```

Do not return the full report content. The report file is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Planning without analysis input | Uninformed approach selection | Read analysis reports first |
| Over-detailed step definitions | Rigid plans break on contact | Plan at appropriate granularity |
| Omitting dependencies | Execution order violations | Map dependencies explicitly |
| Vague success criteria | Cannot assess completion | Define measurable, binary conditions |
| Single approach without alternatives | Missed optimization opportunities | Consider and document alternatives |
| Ignoring constraints | Unachievable plans | Incorporate constraints into approach selection |
| Skipping risk assessment | Unprepared for failures | Identify risks with mitigations |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_before_execution.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

## Sub-Layer Launch

When this objective requires step-level execution, launch the
`planning_procedure` agent. Procedure agents carry the per-procedure
mechanics under 
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/approach_selection.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/checkpoint_definition.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/constraint_mapping.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/risk_assessment.md`
.

Brief template for the `planning_procedure` agent — Execute <procedure-name> within planning objective:

```text
<ctxmod_activate skill="agentic_workflows_planning_procedure" />
Procedure: <procedure-name>
<procedure-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Available procedure agents in the planning domain: dispatch against
the procedure variants in 
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/approach_selection.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/checkpoint_definition.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/constraint_mapping.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/risk_assessment.md`

(approach-selection, dependency-analysis, checkpoint-definition,
risk-assessment, constraint-mapping).
