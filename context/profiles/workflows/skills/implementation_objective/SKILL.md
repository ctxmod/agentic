---
description: "Modify artifacts to achieve specific outcomes — read state, apply targeted changes, verify results — and produce the implementation report at a report path reserved just-in-time via reserve_work_path(type=\"report\")."
binds_to_depth: objective
---

# Implementation Objective

Modify artifacts to achieve specific outcomes. Implementation objectives
read existing state, apply targeted changes, and verify results. They
operate on artifacts identified through discovery and planned through
analysis.

## Variant Selection

Specialized implementation variants add specific methodology to this base
objective. Read the variant matching the assigned scope; variants are
additive, not replacements. Each variant corresponds to one of the
three implementation capability phases that dispatch this objective.

| Phase dispatching | Variant | Read |
|-------------------|---------|------|
| `implement_task_phase` | task (edit) | `reference/edit.md` |
| `implement_fix_phase` | fix (edit) | `reference/edit.md` |
| `implement_refactor_phase` | refactor | `reference/refactor.md` |
| (ad-hoc, any phase) | create | `reference/create.md` |
| (ad-hoc, any phase) | delete | `reference/delete.md` |
| (ad-hoc, any phase) | rename | `reference/rename.md` |
| (ad-hoc, any phase) | direct | `reference/direct.md` |

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation_objective/reference/create.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation_objective/reference/delete.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation_objective/reference/direct.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation_objective/reference/edit.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation_objective/reference/refactor.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation_objective/reference/rename.md" />

Variants must be read when even somewhat relevant.

- the /implement command selection guidance — which implementation
  variant (task, fix, refactor, or ad-hoc) fits the assigned change —
  <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/implement.md" />

## Core Behavior

1. **Read**: Understand current artifact state before modifying
2. **Plan**: Identify minimal changes that achieve the objective
3. **Modify**: Apply changes with precision
4. **Verify**: Confirm changes achieve intended outcome
5. **Report**: Document what changed and why

## Relationship to Discovery and Analysis

Implementation follows discovery and analysis in the workflow:

| Phase | Focus | Output |
|-------|-------|--------|
| Discovery | What exists | Catalogs, inventories, maps |
| Analysis | What to do | Recommendations, priorities, plans |
| **Implementation** | **Do it** | **Modified artifacts** |

Implementation objectives receive targets from analysis. They transform
recommendations into applied changes.

## Pre-Implementation

Before modifying:

1. Read target artifacts to understand current state
2. Understand the specific change required
3. Identify potential side effects or dependencies
4. Confirm scope boundaries
5. Verify tool access for required operations

## During Implementation

Apply changes with these guidelines:

- **Minimal scope**: Change only what the objective requires
- **Context preservation**: Maintain integrity of unchanged content
- **Convention adherence**: Follow existing patterns in the artifact
- **Reference integrity**: Update or preserve cross-references
- **Verification readiness**: Enable outcome confirmation

## Supporting Procedures

Implementation procedures carry the step-level mechanics. Link
conditionally on what the assigned scope requires:

the `implementation_procedure` skill

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Organize and write the implementation report at a report path reserved just-in-time via `ctxmod work new report "<title>"` following the standard report structure.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Implementation reports include these objective-specific sections beyond
the standard structure:

- **Changes**: What was modified, with file:line ranges
- **Rationale**: Why each change was made
- **Verification**: Outcome of post-modification checks
- **Side Effects**: Unintended consequences, if any

Target length: 500-1500 words. Maximize signal-to-noise ratio.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_implementation.md" />

## Tool Constraints

**Allowed**:
- Read: Examine target artifacts
- Edit: Modify existing artifacts
- Write: Create new artifacts when required
- Bash: Execute necessary commands
- Grep/Glob: Search for related content

**Forbidden**:
- Modifying outside specified scope
- Launching sub-agents
- Skipping verification
- Making changes without reading first

## Response Format

After completing implementation, return a brief response:

```
Completed implementation at {TARGET_PATH(S)}.

Summary: [1-2 sentence description of changes made]
```

Include verification results if relevant.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Implementing without reading | Breaks context | Read artifact first |
| Scope creep | Unintended changes | Bound modifications tightly |
| Skipping verification | Unconfirmed outcomes | Validate changes |
| Ignoring conventions | Inconsistent artifacts | Match existing patterns |
| Leaving broken references | Degraded quality | Update all references |
| Mixed objectives | Unclear changes | Use appropriate variant |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile

## Sub-Layer Launch

When this objective requires step-level execution, launch the
`implementation_procedure` agent. Procedure agents carry the
per-procedure mechanics under

- `ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure@workflows/reference/change_planning.md`
- `ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure@workflows/reference/completion_reporting.md`
- `ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure@workflows/reference/context_assessment.md`
- `ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure@workflows/reference/execution_checkpoints.md`
.

Brief template for the `implementation_procedure` agent — Execute <procedure-name> within implementation objective:

```text
<ctxmod_activate skill="implementation_procedure" />
Procedure: <procedure-name>
<procedure-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Available procedure agents in the implementation domain: dispatch
against the procedure variants in

- `ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure@workflows/reference/change_planning.md`
- `ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure@workflows/reference/completion_reporting.md`
- `ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure@workflows/reference/context_assessment.md`
- `ctx:github.com/ctxmod/agentic/context::shared::implementation_procedure@workflows/reference/execution_checkpoints.md`
 (change-planning,
context-assessment, execution-checkpoints, completion-reporting).

## Parent-Layer Dispatch

This objective is dispatched by three sibling phase skills in the
implementation capability, one per capability variant:

| Phase skill | Variant typically dispatched | Reference material |
|-------------|------------------------------|--------------------|
| `implement_task_phase` | task (edit) | `ctx:github.com/ctxmod/agentic/context::shared::implement_task_phase@workflows/reference/task.md` |
| `implement_fix_phase` | fix (edit) | `ctx:github.com/ctxmod/agentic/context::shared::implement_fix_phase@workflows/reference/findings.md` |
| `implement_refactor_phase` | refactor | `ctx:github.com/ctxmod/agentic/context::shared::implement_refactor_phase@workflows/reference/refactor.md` |

Ad-hoc objective-depth agents may also load this skill for general
implementation work outside any of the three capability phase chains.
