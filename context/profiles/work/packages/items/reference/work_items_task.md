# Task Writing Objective

Create a task that defines a granular activity within an issue. Tasks represent the smallest trackable work units—specific activities that contribute to issue completion. Each task describes what to do, provides necessary context, and defines when it is complete. Produce the task document at the designated TASK_PATH.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_items.md" />

## Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_structure_task.md" />

The schema defines record fields, required sections, optional sections, task granularity guidelines, status lifecycle, path conventions, and validation checklist. This objective defines the behavioral process for creating tasks.

## Scope

Tasks represent specific activities:

- File modifications with defined changes
- Verification activities with expected outcomes
- Investigation steps with deliverables
- Review activities with artifacts
- Configuration changes with parameters
- Communication or coordination activities

Tasks are optional within issues. Use tasks when an issue requires progress tracking at finer granularity.

## Principles

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`

Context efficiency keeps task descriptions minimal—include only what enables execution. Reference over duplication encourages linking to parent issues rather than restating completion criteria. KISS ensures tasks remain atomic and focused.

## Core Behavior

1. **Understand**: Clarify what activity this task represents
2. **Scope**: Define the boundaries of this specific task
3. **Contextualize**: Link to parent issue and sibling tasks
4. **Detail**: Provide information needed to complete the activity
5. **Write**: Produce task document at TASK_PATH

## When to Use Tasks

Create tasks when:

- Issue spans multiple distinct activities
- Progress tracking requires granular visibility
- Work parallelizes across activities
- Handoffs occur between activities
- Sequential dependencies exist within the issue

Skip tasks when:

- Issue is already atomic
- Overhead exceeds tracking value
- Activities are trivially obvious

## Task Granularity

Tasks should be atomic—completable in a single focused session.

| Granularity | Example | Appropriate? |
|-------------|---------|--------------|
| Too coarse | "Implement feature" | No—decompose further |
| Appropriate | "Add validation for field X" | Yes—single focused activity |
| Too fine | "Open file Y" | No—combine with related activities |

Target: Tasks completable within hours, not days or minutes.

## Pre-Writing

Before drafting:

1. Review parent issue for context and completion criteria
2. Identify sibling tasks to understand sequencing
3. Gather specific details needed for the activity
4. Confirm TASK_PATH parameter is available

## During Writing

`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`

While authoring:

- Verify the task contributes to parent issue completion
- Include paths to relevant artifacts
- Specify dependencies on other tasks
- Use precise, actionable language
- Keep scope narrow and focused

## Document Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the task document following the report creation and organization procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Task documents follow report creation patterns. Tasks reference their parent issue. Task completion contributes to but does not guarantee issue completion.

## Tool Constraints

**Allowed**:
- Read: Issue, sibling tasks, relevant artifacts, schema
- Glob: Find related files
- Grep: Search for context
- Write: Task document at TASK_PATH only

**Forbidden**:
- Completing the task (separate execution concern)
- Modifying existing artifacts
- Writing outside TASK_PATH

## Response Format

After writing the task, return a brief response:

```
Wrote task to {TASK_PATH}.

Summary: [1-2 sentence summary of activity and dependencies]
```

Do not return the full task content. The document is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Vague activities ("Work on X") | Unclear completion | Specific actions ("Add X to Y") |
| Unbounded scope | Never complete | Define narrow boundaries |
| Missing dependencies | Blocked work | Specify task relationships |
| Excessive granularity | Overhead exceeds value | Combine trivial steps |
| Orphan tasks | Lost context | Link to parent issue |
| Duplicate activities | Wasted effort | Reference shared tasks |
