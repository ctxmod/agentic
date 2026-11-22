---
description: "Structure the approach for subsequent phases based on discovery and analysis findings, transforming understanding into actionable strategy with defined steps, dependencies, and success criteria."
binds_to_depth: phase
---

# Planning Phase

Structure the approach for subsequent phases based on discovery and
analysis findings. Planning transforms understanding into actionable
strategy with defined steps, dependencies, and success criteria.

Planning is a **phase-rooted partial domain**: there is no parent
`planning-workflow` skill (decision 2 — sparse cell omitted). The
phase skill stands on its own; planning contexts are dispatched
directly into this phase rather than reached via a workflow.

## When to Use

Use planning phase when:

- Execution requires coordinated steps
- Dependencies between actions must be sequenced
- Multiple approaches exist and selection is needed
- Success criteria must be defined before action
- Risk mitigation strategies should be established

Planning bridges understanding and action. It converts findings into
executable strategy.

## Variant Selection

Read the variant file matching the assigned task. Do not read all six —
each is independently applicable and reading extras wastes context.

| Planning scenario | Read |
|-------------------|------|
| Single recommended approach, standard execution | `reference/general.md` |
| Implementing fixes based on validation findings | `reference/correction.md` |
| Multiple viable approaches require explicit comparison | `reference/multi_option.md` |
| Story and issue organization from analysis findings | `reference/work_item.md` |
| Phased transition between states with rollback | `reference/migration.md` |
| Recommendations or options requiring human judgment | `reference/human_approval.md` |

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_phase/reference/correction.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_phase/reference/general.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_phase/reference/human_approval.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_phase/reference/migration.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_phase/reference/multi_option.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_phase/reference/work_item.md" />

## Model Selection

**Primary**: Sonnet

Planning requires organizing information and structuring approaches.
Sonnet handles this well.

**Exception**: Use Opus for planning involving complex trade-offs,
architectural decisions, or novel problem domains requiring deep
reasoning.

## Coordination Pattern

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />

Planning typically follows sequential pattern, proceeding after
discovery and analysis complete. Planning requires their output as
input.

For complex planning requiring multiple perspectives, parallel analysis
agents may inform a sequential planning phase.

## Context Requirements

**Parameters provided by main agent**:

| Parameter | Required | Purpose |
|-----------|----------|---------|
| Report (reserved) | Yes | Sub-agent creates the report record just-in-time with `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing |
| INPUT_REPORT | Yes | Analysis report to plan from |
| Work item context | If applicable | EPIC_PATH, STORY_PATH, ISSUE_PATH |

## Transition Criteria

Planning completes and transitions when:

- All required steps are defined
- Dependencies are clear and achievable
- Success criteria are measurable
- Risks are identified with mitigations
- Plan is actionable by subsequent phases

If planning reveals blockers, escalate before proceeding.

## Supporting Objective and Procedures

The planning objective carries the analytical methodology — approach
selection, dependency mapping, checkpoint definition, risk assessment,
constraint mapping. The planning procedure carries the step-level
mechanics. Link conditionally on what the assigned variant requires.

the `planning_objective` skill
the `planning_procedure` skill

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Planning without discovery | Uninformed plans fail | Complete discovery first |
| Over-detailed planning | Wasted effort on uncertain details | Plan at appropriate granularity |
| Ignoring constraints | Unachievable plans | Incorporate constraints explicitly |
| Single approach | May miss better alternatives | Consider multiple approaches |
| Vague success criteria | Cannot assess completion | Define measurable criteria |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_before_execution.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`planning_objective` agent. Objective agents carry the analytical
methodology under 
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/constraint_based.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/decision_map.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/general.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/incremental.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/phased.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/risk_aware.md`
.

Brief template for the `planning_objective` agent — Produce <planning-scenario> plan within planning phase:

```text
<ctxmod_activate skill="agentic_workflows_planning_objective" />
Scenario: <planning-scenario>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Available objective agents in the planning domain: dispatch against the
scenario variants in 
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/constraint_based.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/decision_map.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/general.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/incremental.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/phased.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/risk_aware.md`
 (general,
constraint-based, incremental, phased, risk-aware).
