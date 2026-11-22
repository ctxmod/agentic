# Implement Command Selection Guidance

Selection guidance for the `/implement` command — the unified entry
point for artifact modifications through task execution, correction
application, or behavior-preserving reorganization. This document
captures variant-selection logic, comparison matrices, and
"which command when" guidance. Execution lives in the implementation
phase skills (`implement_task_phase`, `implement_fix_phase`,
`implement_refactor_phase`) and the shared
`implementation_objective`/`implementation_procedure` layers.

## Variant Comparison

`/implement [variant] [scope] [options]` routes to one of three
sibling capability phase skills based on the variant argument.

| Input | Routes To | Phase Skill |
|-------|-----------|-------------|
| `/implement task` | `/implement-task` | `implement_task_phase` |
| `/implement fix ...` | `/implement-fix` | `implement_fix_phase` |
| `/implement refactor ...` | `/implement-refactor` | `implement_refactor_phase` |

## Variant Characteristics

### Task Execution

Execute work defined in a task work item.

| Aspect | Value |
|--------|-------|
| Command | `/implement-task` |
| Phase skill | `implement_task_phase` |
| Work Item Context | Required (task) |
| Input | Task acceptance criteria |
| Output | Modified artifacts, verification report |

### Validation Fixes

Apply corrections from validation findings.

| Aspect | Value |
|--------|-------|
| Command | `/implement-fix` |
| Phase skill | `implement_fix_phase` |
| Work Item Context | Optional |
| Input | Validation report or artifact path |
| Output | Corrected artifacts, re-validation report |

### Refactoring

Reorganize artifacts without behavior change.

| Aspect | Value |
|--------|-------|
| Command | `/implement-refactor` |
| Phase skill | `implement_refactor_phase` |
| Work Item Context | Required (issue or story) |
| Input | Scope and refactor type |
| Output | Transformed artifacts, equivalence verification |

## Work Item Context by Variant

| Variant | Work Item Context | Rationale |
|---------|-------------------|-----------|
| task | Required | Task provides acceptance criteria and scope boundaries |
| fix | Optional | Reports nest in work item if provided |
| refactor | Required | Tracks scope boundaries and enables reversibility |

## Variant-Specific Discovery and Verification

| Variant | Discovery Focus | Planning Depth | Verification Emphasis |
|---------|-----------------|----------------|----------------------|
| task | Task context, acceptance criteria | Moderate | Acceptance criteria pass |
| fix | Validation findings, failure context | Lightweight | Re-validation clean |
| refactor | Baseline state, consumers | Thorough | Behavioral equivalence |

## Comparison with Specialized Commands

| Command | When to Use |
|---------|-------------|
| `/implement` | General entry point, routing based on context |
| `/implement-task` | Executing defined task work |
| `/implement-fix` | Applying validation corrections |
| `/implement-refactor` | Reorganizing without behavior change |

## Common Characteristics

All implementation commands share these characteristics:

- **Implementation phase chain**: Discovery → Planning → Modification → Verification
- **Verification loops**: Maximum 3 remediation iterations
- **Opus for modification**: Complex changes use Opus model
- **Report persistence**: Phase reports enable progress tracking
- **Atomicity**: Complete or rollback, no partial states

## Modification Quality Standards

All modifications must pass verification:

- No error-severity violations introduced
- All references remain intact
- No unintended changes outside scope
- Variant-specific criteria met

**Variant-specific quality criteria**:

| Variant | Additional Criteria |
|---------|---------------------|
| task | Acceptance criteria satisfied |
| fix | All findings addressed, no new violations |
| refactor | Behavioral equivalence verified |

## Notes

For frequent use, prefer the specialized commands which provide
focused context and skip routing overhead. When variant is ambiguous
from user input, the main agent should clarify before proceeding.
Task implementation requires task context; refactoring requires issue
or story context.

Implementation differs from writing: writing creates new artifacts
while implementation modifies existing ones. For new artifact
creation, use `/write` commands.

## Execution Pointers

- Task execution → launch `implement_task_phase` phase agent
- Validation fixes → launch `implement_fix_phase` phase agent
- Refactoring → launch `implement_refactor_phase` phase agent

Each variant dispatches through its phase-depth capability skill,
which loads `implementation_objective` and `implementation_procedure`
for sub-agent mechanics. See
implementation_objective (the `implementation_objective` skill)
and `ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`.
