---
description: "Step-level planning mechanics — approach selection, dependency analysis, checkpoint definition, risk assessment, and constraint mapping procedures."
binds_to_depth: procedure
---

# Planning Procedures

Execute a single, bounded planning procedure and return its result.
Each procedure below is a self-contained set of prerequisites, steps,
and decision points; the variant material lives in this skill's
`reference/` directory.

## Procedure Selection

Read the reference file matching the assigned task. Do not read all
five — each is independently applicable and reading extras wastes
context.

| Assigned task | Read |
|---------------|------|
| Select optimal execution approach from alternatives | `reference/approach-selection.md` |
| Map execution order and identify critical paths | `reference/dependency-analysis.md` |
| Define verification points with rollback triggers | `reference/checkpoint-definition.md` |
| Evaluate probability/impact, define mitigations | `reference/risk-assessment.md` |
| Define feasible solution space within boundaries | `reference/constraint-mapping.md` |

### Approach Selection

Select the optimal execution approach from alternatives identified
during analysis. Score candidates against weighted criteria, assess
trade-offs, and validate the selection against constraints.

For approach selection, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_procedure/reference/approach_selection.md" />

### Dependency Analysis

Map relationships between planned changes to determine execution
order. Identify data dependencies, structural dependencies, and
external prerequisites. Produce a dependency graph with critical path.

For dependency analysis, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_procedure/reference/dependency_analysis.md" />

### Checkpoint Definition

Define verification points within the execution plan where progress
can be confirmed and rollback remains straightforward. Establish
criteria for proceeding past each checkpoint.

For checkpoint definition, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_procedure/reference/checkpoint_definition.md" />

### Risk Assessment

Evaluate execution risks across probability and impact dimensions.
Define mitigations for identified risks and contingency plans for
high-impact scenarios. Document risk tolerance thresholds.

For risk assessment, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_procedure/reference/risk_assessment.md" />

### Constraint Mapping

Identify boundaries that limit the feasible solution space. Map
resource constraints, time constraints, capability limitations, and
external dependencies. Determine which constraints are negotiable.

For constraint mapping, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::planning_procedure/reference/constraint_mapping.md" />

## Procedure Selection by Planning Complexity

Not every plan requires all five procedures. Match procedure selection
to planning complexity.

| Planning Complexity | Recommended Procedures |
|---------------------|------------------------|
| Single clear approach, few changes | Dependency analysis, checkpoint definition |
| Multiple viable approaches | Approach selection, dependency analysis, checkpoint definition |
| High-risk or uncertain execution | Approach selection, risk assessment, checkpoint definition |
| Tight resource or time constraints | Constraint mapping, approach selection, dependency analysis |
| Complex multi-phase execution | All five procedures |

## Decision Points

**Analysis recommends a single approach**: Skip approach selection.
Proceed directly to dependency analysis using the recommended approach.

**Changes are independent with no ordering constraints**: Skip
dependency analysis. Define checkpoints based on logical groupings
rather than dependency ordering.

**Low-risk execution with clear rollback**: Reduce checkpoint density.
A start checkpoint and end checkpoint may suffice when intermediate
states carry minimal risk.

**No external constraints identified**: Skip constraint mapping.
Internal constraints (dependencies, sequencing) are handled by
dependency analysis.

**Planning reveals gaps in analysis**: Return to analysis phase before
completing the plan. Plans built on incomplete understanding produce
unreliable execution strategies.

## Execution Discipline

- Read only the variant matching the assigned task; do not read all
  five.
- Execute only the assigned procedure; do not expand into adjacent
  steps.
- Report results to the caller-provided path; return a summary, not
  the full body.
- Reference findings with `file:line` ranges rather than inlining
  content.
- Note explicitly when a procedure's prerequisites are unmet rather
  than improvising a substitute.

## Success Criteria

- Execution approach selected with documented rationale
- Dependencies mapped and execution order derived
- Checkpoints defined at stable intermediate states
- Risks identified with mitigations for high-probability or high-impact items
- Constraints documented and incorporated into plan structure
- Plan is actionable by subsequent implementation phases

## Common Issues

**Over-planning simple work**: Applying all five procedures to
straightforward changes wastes effort. Match procedure depth to work
complexity using the selection table above.

**Under-planning complex work**: Skipping dependency analysis or risk
assessment for multi-phase execution leads to blocked steps and
unmitigated failures discovered mid-execution.

**Plans disconnected from analysis**: Plans that introduce approaches
not grounded in analysis findings lack supporting evidence. Every plan
element should trace to an analysis recommendation or discovery
finding.

**Static plans for dynamic execution**: Plans that assume perfect
knowledge fail when execution reveals new information. Include
adaptation triggers — conditions that warrant plan revision rather
than continued execution of an outdated strategy.

**Checkpoint-free execution**: Plans without intermediate verification
points force all-or-nothing outcomes. Even simple plans benefit from
at least one mid-execution checkpoint.

## Workflow Context

This procedure serves the planning objective and phase skills. There
is no `planning-workflow` skill (decision 2 — sparse cell omitted);
planning contexts dispatch directly into the phase or objective.

the `planning_phase` skill
the `planning_objective` skill

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
