# Constraint-Based Planning Objective

Plan execution within explicit boundaries imposed by time, resources, dependencies, or compatibility requirements. Constraint-based planning maps the feasible solution space before sequencing steps, ensuring plans satisfy all hard constraints and optimize across soft constraints.

## Base Behavior

the `planning_objective` skill

## Scope

Constraint-based planning applies when external factors bound the available options:

- Fixed deadlines limiting available execution time
- Resource ceilings (personnel, compute, budget, token budget)
- Dependency locks requiring specific ordering or versions
- Compatibility requirements with existing artifacts or systems
- Regulatory or policy mandates constraining approach selection
- Concurrent work creating contention for shared resources

Use this variant when constraints actively shape the plan. If constraints exist but do not meaningfully restrict options, use the general planning variant.

## Relationship to Other Planning Types

| Type | Focus | Constraint Role |
|------|-------|-----------------|
| General | Single recommended approach | Constraints noted, not central |
| Phased | Multi-phase sequencing | Phase gates may impose constraints |
| Incremental | Iterative adaptation | Constraints may shift between iterations |
| Risk-Aware | Uncertainty mitigation | Constraints inform risk severity |
| **Constraint-Based** | **Boundary satisfaction** | **Constraints drive plan structure** |

## Core Behavior

1. **Inventory**: Catalog all constraints with type, source, and rigidity (hard vs soft)
2. **Classify**: Distinguish hard constraints (non-negotiable) from soft constraints (preferences with trade-offs)
3. **Map**: Define the feasible solution space that satisfies all hard constraints
4. **Evaluate**: Score feasible approaches against soft constraints and analysis recommendations
5. **Optimize**: Select the approach that best satisfies soft constraints within hard boundaries
6. **Sequence**: Order steps to maintain constraint satisfaction throughout execution
7. **Report**: Document constraint map, trade-offs, and selected approach at the reserved report path

## Constraint Taxonomy

Constraints fall into distinct categories that inform handling strategy:

| Category | Examples | Typical Rigidity |
|----------|----------|------------------|
| Temporal | Deadlines, time windows, scheduling conflicts, publication dates | Hard |
| Resource | Budget caps, personnel availability, compute limits, word count limits | Mixed |
| Dependency | Ordering requirements, prerequisite completions, external approvals | Hard |
| Compatibility | Backward compatibility, format requirements, audience expectations, standards conformance | Hard |
| Quality | Accuracy thresholds, coverage targets, consistency floors, readability scores | Soft |
| Policy | Regulatory compliance, organizational standards, access controls, ethics requirements | Hard |
| Methodological | Research protocols, review procedures, style guides, citation standards | Mixed |

Hard constraints eliminate options. Soft constraints rank remaining options.

## Methodology

### Phase 1: Constraint Discovery

Gather constraints from analysis reports, project parameters, and environmental factors. For each constraint, record:

- **Source**: Where the constraint originates (analysis report, policy, deadline)
- **Boundary**: The specific limit or requirement
- **Rigidity**: Hard (violation = failure) or soft (violation = suboptimal)
- **Verification**: How satisfaction will be confirmed

Brief template for a procedure-depth agent (launched from objective depth):

```text
Map the feasible solution space by cataloging each constraint's source, boundary, rigidity, and verification mechanism.
`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/constraint_mapping.md`
```

### Phase 2: Feasibility Assessment

Test whether any approach satisfies all hard constraints simultaneously. When hard constraints conflict, escalate: no feasible plan exists without relaxing at least one constraint. Document which constraint relaxation yields the most viable path.

### Phase 3: Trade-off Analysis

Among feasible approaches, evaluate trade-offs across soft constraints. A plan that exceeds one quality target while meeting another at minimum threshold may outperform balanced alternatives. Document the trade-off rationale for the selected balance.

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/approach_selection.md`

### Phase 4: Constraint-Aware Sequencing

Order execution steps so that constraint satisfaction holds at every intermediate state. Early steps should not consume resources needed by later critical-path items. Place constraint-verification checkpoints before irreversible steps.

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`
`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/checkpoint_definition.md`

## Pre-Constraint Planning

Before planning:

1. Read analysis reports to understand recommended approaches
2. Extract all constraints mentioned in analysis findings and project parameters
3. Verify constraint sources and confirm boundaries with available evidence
4. Identify constraints that interact or conflict with each other
5. Determine which constraints have verification mechanisms
6. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

## During Constraint Planning

- **Hard constraints first**: Establish feasibility before optimizing
- **Explicit trade-offs**: Document what each approach sacrifices
- **Constraint monitoring**: Define how constraint satisfaction will be verified during execution
- **Buffer allocation**: Reserve margin for constraints with uncertainty (time buffers, resource reserves)
- **Escalation paths**: Define response when constraints tighten or new constraints emerge

## Verification

After completing the plan:

1. Confirm every hard constraint has a corresponding satisfaction mechanism in the plan
2. Verify the execution sequence maintains constraint satisfaction at each step
3. Check that soft constraint trade-offs are documented with rationale
4. Validate that checkpoint criteria include constraint verification
5. Ensure contingency responses address constraint violations

## Tool Constraints

**Allowed**:
- Read: Analysis reports, constraint documentation, reference files
- Grep: Search for constraint references and dependency information
- Glob: Locate artifacts affected by constraints
- Write: Planning report at the reserved report path only

**Forbidden**:
- Modifying artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Relaxing hard constraints without documenting escalation

## Response Format

After writing the report, return a brief response:

```
Wrote constraint-based planning report to <reserved report path>.

Summary: [2-3 sentences covering key constraints, selected approach, and primary trade-offs]
```

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Treating soft constraints as hard | Unnecessarily restricts solution space | Classify rigidity explicitly |
| Ignoring constraint interactions | Cascading violations | Map constraint dependencies |
| Planning without feasibility check | Unachievable plans | Verify hard constraint satisfaction first |
| Omitting trade-off documentation | Opaque decisions | Record what each approach sacrifices |
| Assuming static constraints | Plans break when boundaries shift | Define monitoring and escalation |
| Over-optimizing soft constraints | Fragile plans with no margin | Reserve buffers for uncertainty |
| Skipping intermediate verification | Late discovery of violations | Place checkpoints before irreversible steps |

## Related

the `planning_objective` skill

Brief template for a procedure-depth agent (launched from objective depth):

```text
Apply constraint mapping and approach selection to define the feasible solution space and choose the optimal path within it.
`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/constraint_mapping.md`
`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/approach_selection.md`
```

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/risk_assessment.md`

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
