---
description: "Task implementation phase — execute work defined in a task work item through discovery, planning, modification, and verification with embedded remediation."
binds_to_depth: phase
---

# Implement Task Phase

Task implementation phase — execute work defined in a task work item.
This is one of three sibling phase skills in the implementation
capability (task, fix, refactor); they share the same objective
(`implementation_objective`) and procedure (`implementation_procedure`)
layers and differ only in the workflow reference material they
dispatch against.

Task implementation is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/implement_task` slash-command,
the `/implement` router, or any other domain's workflow agent
references this phase as a "implement task" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Executing work defined in a task work item
- Task provides scope, acceptance criteria, and context
- Implementation must satisfy task requirements
- Work item tracking and reporting required

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a five-step
sequence. The same objective (`implementation_objective`) runs each
step; the phase-specific reference material below scopes each dispatch.

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

### Phase 1: Task Resolution

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_task_resolution.md" />

- Report: `{TASK_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-task-resolution.md`

**Workflow-specific notes**:
- Loads task definition from work item
- Extracts scope, acceptance criteria, related artifacts
- Identifies dependencies on other tasks
- Establishes verification criteria for Phase 5

### Phase 2: State Discovery

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_general.md" />

- Report: `{TASK_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-state-discovery.md`

**Workflow-specific notes**:
- Discovers current state of artifacts identified in task
- Focuses on areas relevant to task requirements
- Documents existing patterns and conventions
- May skip if task resolution provides sufficient context

### Phase 3: Change Planning

`ctx:github.com/ctxmod/agentic/context::shared::analyze_gap_phase@workflows/reference/gap.md`
`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/general.md`

- Report: `{TASK_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-implementation-plan.md`

**Workflow-specific notes**:
- Gap analysis identifies what must change (current vs. task requirements)
- Planning produces change plan with affected artifacts
- Plans minimal changes achieving task acceptance criteria
- Identifies verification strategy for each change

### Phase 4: Implementation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implement_task_phase/reference/task.md" />

- Report: `{TASK_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-implementation.md`

**Workflow-specific notes**:
- Applies planned changes according to plan
- Follows existing patterns and conventions
- Uses Opus model for modification quality
- Does not expand scope beyond task requirements

### Phase 5: Verification

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_general.md" />
`ctx:github.com/ctxmod/agentic/context::shared::remediation_phase@workflows/reference/general.md`

- Report: `{TASK_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-verification.md`

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

Main agent passes report **paths** between phases, never report content.

## Execution Strategy

1. **Resolve task**: Resolve TASK_PATH from `--task` parameter or context
2. **Task Resolution**: Launch task resolution sub-agent; wait for completion
3. **State Discovery**: Launch discovery sub-agent; wait for completion
4. **Change Planning**: Launch planning sub-agent with discovery output
5. **Implementation**: Launch implementation sub-agent with plan; wait
6. **Verification Loop**:
   a. Launch verification sub-agent targeting modified artifacts
   b. Read verification report
   c. If issues found: Launch correction implementation, re-verify (max 3)
   d. If clean: Exit loop
7. **Complete**: Return implementation summary and verification status

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

Main agent creates sub-tasks, tasks execute sequentially with each
reading all previous task reports as compulsory reads.

## Error Handling

| Situation | Response |
|-----------|----------|
| Task cannot be resolved | Report resolution failure, await user decision |
| Implementation fails | Document partial changes, assess rollback, await decision |
| Verification exceeds iterations | Report persistent issues, await user decision |

Error handling follows stop-report-propose-await pattern.

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

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`implementation_objective` agent. The objective in turn launches
`implementation_procedure` for step mechanics.

Brief template for the `implementation_objective` agent — Execute <phase-name> within task implementation:

```text
<ctxmod_activate skill="implementation_objective" />
Variant: task
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"implement task" as a sub-step. Launch the `implement_task_phase` agent
from any workflow that needs task-scope implementation.

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every implementation
phase variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_phase_selection.md" />

For a single-scope implementation that does not need the full five-phase
chain, dispatch the general execution template directly:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_general.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_parallel.md" />

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped implementation guidance for the sub-agents this phase launches:

the `implementation_objective` skill
the `implementation_procedure` skill
