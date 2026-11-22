# Task Implementation Workflow

Variant of implementation workflow for executing work defined in task work items. Includes task resolution phase and verification against task acceptance criteria.

## Workflow Overview

```
Phase 1: Task Resolution
         ↓
Phase 2: State Discovery
         ↓
Phase 3: Change Planning
         ↓
Phase 4: Implementation ←───────┐
         ↓                      │
Phase 5: Verification           │
         ↓                      │
    Issues Found? ─────Yes──────┘
         ↓ No
      Complete
```

## When to Use

Use task implementation workflow when:

- Executing work defined in a task work item
- Task provides scope, acceptance criteria, and context
- Implementation must satisfy task requirements
- Work item tracking and reporting required

## Phase Structure

### Phase 1: Task Resolution

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_task_resolution.md" />

**Workflow-specific notes**:
- Loads task definition from work item
- Extracts scope, acceptance criteria, related artifacts
- Identifies dependencies on other tasks
- Establishes verification criteria for Phase 5

### Phase 2: State Discovery

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_general.md" />

**Workflow-specific notes**:
- Discovers current state of artifacts identified in task
- Focuses on areas relevant to task requirements
- Documents existing patterns and conventions
- May skip if task resolution provides sufficient context

### Phase 3: Change Planning

`ctx:github.com/ctxmod/agentic/context::shared::analyze_gap_phase@workflows/reference/gap.md`
`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/general.md`

**Workflow-specific notes**:
- Gap analysis identifies what must change (current vs. task requirements)
- Planning produces change plan with affected artifacts
- Plans minimal changes achieving task acceptance criteria
- Identifies verification strategy for each change

### Phase 4: Implementation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_edit.md" />

**Workflow-specific notes**:
- Applies planned changes according to plan
- Follows existing patterns and conventions
- Uses Opus model for modification quality
- Does not expand scope beyond task requirements

### Phase 5: Verification

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_general.md" />
`ctx:github.com/ctxmod/agentic/context::shared::remediation_phase@workflows/reference/general.md`

**Workflow-specific notes**:
- Verifies implementation satisfies task acceptance criteria
- Checks for unintended side effects
- Verifies reference integrity
- Validation loop (max 3 iterations) until criteria met

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Phase 1: Task Resolution
- Receives: TASK_PATH
- Produces: {TASK_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-task-resolution.md

Phase 2: State Discovery
- Receives: TASK_PATH, Phase 1 output
- Produces: {TASK_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-state-discovery.md

Phase 3: Change Planning
- Receives: TASK_PATH, Phase 1 output, Phase 2 output
- Produces: {TASK_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-implementation-plan.md

Phase 4: Implementation
- Receives: TASK_PATH, Phase 3 output
- Available: Phase 2 output (context reference)
- Produces: {TASK_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-implementation.md

Phase 5: Verification
- Receives: TASK_PATH, modified artifacts, Phase 1 output (criteria)
- Produces: {TASK_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-verification.md
```

## Workflow Variants

### Lightweight Task

Skip Phase 2 (state discovery) when:
- Task resolution provides sufficient context
- Single artifact requires modification
- Scope is tightly bounded

```
Task Resolution → Change Planning → Implementation → Verification
```

### Well-Specified Task

Skip Phase 3 (planning) when:
- Task defines exact implementation steps
- Minimal judgment required
- Scope is clear and bounded

```
Task Resolution → State Discovery → Implementation → Verification
```

### Complex Task

Decompose into sub-tasks when:
- Task spans multiple independent concerns
- Single workflow would be inefficient
- Planning recommends decomposition

Main agent creates sub-tasks, tasks execute sequentially with each reading all previous task reports as compulsory reads.

## Error Handling

### Task Resolution Failure

If task cannot be resolved:

1. Report resolution failure with context
2. Check task definition validity
3. Propose scope clarification or task revision
4. Await user decision

### Implementation Failure

If implementation fails:

1. Report implementation failure with context
2. Document partial changes if any
3. Assess rollback necessity
4. Propose recovery or alternative approach
5. Await user decision

### Verification Failure (Exceeded Iterations)

If verification fails after maximum iterations:

1. Report persistent issues with remediation history
2. Document what remains unresolved
3. Compare to task acceptance criteria
4. Propose manual intervention or task revision
5. Await user decision

## Quality Criteria

| Criterion | Validation Method |
|-----------|-------------------|
| Acceptance criteria met | Phase 5 verifies against task criteria |
| No unintended side effects | Phase 5 checks scope boundaries |
| Reference integrity | Phase 5 validates references |
| Pattern compliance | Phase 5 checks conventions |

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping task resolution | Missing requirements context | Resolve task first |
| Expanding scope beyond task | Scope creep, iteration exhaustion | Implement task scope only |
| Skipping verification | Unconfirmed task completion | Verify against criteria |
| Implementing without planning | Uncontrolled changes | Plan changes first |
