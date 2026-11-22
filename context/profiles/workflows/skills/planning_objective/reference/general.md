# General Planning Objective

Translate a single recommended approach from analysis into an executable step sequence. General planning applies when analysis produces one clear path forward without phased rollouts, iterative feedback loops, or elevated risk profiles.

## Base Behavior

the `planning_objective` skill

## Scope

General planning applies when:

- Analysis identifies one recommended approach with clear rationale
- Execution follows a single linear or lightly branching sequence
- Dependencies are straightforward and well-understood
- Risk level does not warrant dedicated mitigation planning
- Constraints are known and already factored into the analysis recommendation

General planning does not apply when execution spans distinct phases with gates, requires iterative adaptation, operates under elevated uncertainty, or faces tight resource constraints. Use specialized variants for those scenarios.

## Core Behavior

1. **Validate**: Confirm analysis input completeness and approach clarity
2. **Decompose**: Break the selected approach into ordered execution steps
3. **Sequence**: Arrange steps to respect dependencies and enable parallelization where possible
4. **Checkpoint**: Define verification points between steps to confirm progress
5. **Criteria**: Establish measurable success conditions for overall plan completion
6. **Report**: Write the plan to the reserved report path
7. **Summarize**: Return brief summary with report path

## Relationship to Other Planning Types

| Type | When to Use | Complexity |
|------|-------------|------------|
| **General** | **Single clear approach, standard execution** | **Low to moderate** |
| Phased | Multi-phase execution with gates and handoffs | Moderate to high |
| Incremental | Iterative cycles with adaptation points | Moderate |
| Risk-Aware | Elevated uncertainty or high-impact failures | High |
| Constraint-Based | Tight resource, time, or capability boundaries | Variable |

General planning is the default. When a scenario fits none of the specialized variants, use general planning.

## Pre-Planning

Before planning:

1. Read all analysis reports referenced in the prompt
2. Confirm a single recommended approach exists with supporting rationale
3. Identify the artifacts affected by the approach
4. List known dependencies between planned changes
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

When the planning context references work-store items, use `ctxmod work show <id>` (metadata, parent, status) and `ctxmod work list --parent <id>` / `--kind report --owner <id>` (children and report paths) instead of reading the work item file and globbing reports manually.

`work_store_first` in the ctxmod builtin `work_discipline` profile

If analysis presents multiple competing approaches without a clear recommendation, perform approach selection before proceeding.

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/approach_selection.md`

## During Planning

Apply these guidelines while constructing the plan:

- **Step granularity**: Each step should represent one verifiable unit of work, neither too granular (micro-tasks that add coordination overhead) nor too coarse (steps that hide internal complexity)
- **Dependency mapping**: State which steps depend on prior completions and which can execute in parallel
- **Checkpoint placement**: Insert verification after steps that produce artifacts consumed downstream
- **Success conditions**: Define binary pass/fail criteria for each checkpoint and the overall plan
- **Attribution**: Link each step back to the analysis finding that motivates it

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`
`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/checkpoint_definition.md`

## Verification

After producing the plan:

1. Confirm every analysis recommendation maps to at least one plan step
2. Verify dependency ordering contains no circular references
3. Check that checkpoints cover all steps producing artifacts for downstream consumption
4. Validate success criteria are measurable and binary
5. Ensure the plan remains achievable within known constraints

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the planning report path just-in-time and create the structured output following the report organization and creation procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

### General Planning Report Sections

Beyond standard report structure, general planning reports include:

- **Approach**: The selected strategy with rationale referencing analysis findings
- **Execution Plan**: Numbered steps with dependencies, parallel opportunities, and responsible scope
- **Checkpoints**: Verification points with pass/fail criteria and rollback triggers
- **Success Criteria**: Measurable conditions for plan completion

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

Summary: [2-3 sentence summary of the approach, step count, and key checkpoints]
```

Do not return the full report content. The report file is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Planning without reading analysis | Uninformed decisions | Consume all analysis input first |
| Work items without dependencies stated | Execution order ambiguity | Map dependencies explicitly |
| Missing checkpoints | Failures detected late | Verify after artifact-producing work items |
| Vague success criteria | Completion undecidable | Define binary, measurable conditions |
| Over-decomposing work items | Coordination overhead | One verifiable unit per work item |
| Ignoring parallel opportunities | Slower execution | Identify independent work items |

## Related

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
