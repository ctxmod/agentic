# Task Work Item Schema

Tasks define atomic activities within issues. They represent the smallest trackable work units—specific actions that contribute to issue completion. Tasks are optional; use them when an issue requires granular progress tracking or when work parallelizes across distinct activities.

## Record Fields

A task's state lives in its work-store record, never in the body: the store writes the managed fields, and `ITEM.md` carries no managed metadata (frontmatter in a body is author text the store neither reads nor splits). Create the record and set unmanaged fields at creation:

```sh
ctxmod work new task "<title>" --parent <parent id> [--set responsible=<assignee identifier>]
```

| Field | Required | Set by | Description |
|-------|----------|--------|-------------|
| id | Yes | store (`ctxmod work new`) | Ten-letter record id; legacy-style labels (T01, T02, etc.) may appear in titles but never replace the id |
| kind | Yes | store | Always `task` |
| status | Yes | `new`, `set` | Current lifecycle state (see Status Lifecycle) |
| created | Yes | store | Creation instant (RFC 3339 UTC) |
| parent | Yes | `new --parent`, `set parent=` | Parent issue identifier — the parent's record id |
| responsible | No | `--set responsible=` | Responsible party (unmanaged; the store's own `owner` field names the item an artifact was created under) |
| title | Yes | `new`, `set` | One-line title; mirrors the body's H1 |

Tasks always belong to an issue. The parent field is required.

## Required Sections

### Title

Action phrase describing the activity.

**Format**: Imperative verb phrase

**Examples**:
- "Define Required Fields for Report Artifacts"
- "Verify Configuration Loading Order"
- "Document Validation Rule Format"

### Summary

1-2 sentences describing what this task accomplishes.

**Content requirements**:
- What action completes this task
- How it contributes to parent issue

Keep summaries minimal—context comes from the parent issue.

### Completion Criteria

Single condition indicating task completion.

```markdown
## Completion Criteria

{Verifiable condition that defines "done"}
```

**Criteria characteristics**:
- Single, focused condition when possible
- Binary outcome (done or not done)
- Scoped to this task alone

### Context

Information needed to perform the activity.

```markdown
## Context

**Relevant artifacts**:
- {Path or reference}
- {Path or reference}

**Dependencies**: {Tasks that must complete first}
```

## Optional Sections

### Approach

Suggested method when not obvious. Use sparingly—tasks should be atomic enough that approach is evident.

### Notes

Additional considerations affecting the activity.

### Artifacts

Items produced by this task:

```markdown
## Artifacts

| Artifact | Type | Location |
|----------|------|----------|
| {What is produced} | {Type} | {Path} |
```

## Task Granularity

Tasks should be atomic—completable in a single focused session.

| Granularity | Example | Appropriate? |
|-------------|---------|--------------|
| Too coarse | "Complete all validation rules" | No—decompose into issue |
| Appropriate | "Define rules for section presence" | Yes—single focused activity |
| Too fine | "Open reference file" | No—combine with related work |

**Guidance**: Tasks should be substantial enough to warrant tracking but small enough to complete without interruption.

## Task Dependencies

Specify dependencies between tasks when order matters.

```markdown
## Context

**Dependencies**: Requires T01 completion before start
```

| Dependency Type | Description |
|-----------------|-------------|
| Blocked by | Cannot start until another completes |
| Enables | Completion allows another to start |
| Parallel | Can proceed simultaneously |

Tasks without explicit dependencies can proceed in parallel.

## Status Lifecycle

Statuses are the store's item vocabulary (`ctxmod work set <id> status=<status>`); the store accepts any order, so the transitions below are this module's doctrine.

| Status | Meaning | Transitions To |
|--------|---------|----------------|
| `open` (pending) | Ready to start | in_progress, blocked |
| `in_progress` | Work underway | done, blocked |
| `blocked` | Cannot proceed | in_progress, open |
| `done` (completed) | Criterion met | (terminal) |

Tasks use a simplified lifecycle without a separate draft stage or cancellation: a new task starts `open` and is ready to start. Draft tasks should remain in the parent issue until ready. Cancelled work removes the task entirely (archive it with `ctxmod work archive <id>`) rather than setting `cancelled`.

## Path Convention

A task is an item record; its body and everything it owns live in its item directory:

```
items/<YYYY-MM>/<DD-HHMM>-<id>-<slug>/   # under the store directory (`ctxmod state show`)
├── ITEM.md                              # This document
├── reports/                             # Task-scoped reports (created with --owner <id>)
│   └── <YYYY-MM>/
│       └── <DD-HHMM>-<report id>-<slug>.md
└── ...                                  # intents/, plans/, … for other owned records
```

Items never nest physically: the hierarchy is the `parent` field (a task's parent is an issue (or any other item)), and `ctxmod work set <id> parent=<id>` re-parents without moving a file.

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

## Example

```markdown
<!-- record state (T01): kind=task status=open parent=<id of I01> responsible=validation-team -->

# Define Required Fields for Report Artifacts

## Summary

Enumerate which fields are mandatory for each report artifact type. This information feeds into the validation rules being developed in the parent issue.

## Completion Criteria

Required field definitions exist for all six report types with clear presence requirements.

## Context

**Relevant artifacts**:
- Report schemas: 
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_implementation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_plan.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_remediation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_research.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_summary.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`

- Frontmatter schema: `ctx:github.com/ctxmod/writing/context::shared::metadata@documentation/reference/frontmatter.md`

**Dependencies**: None—can start immediately

## Approach

Review each report schema. Extract required fields from frontmatter tables. Document in consolidated format suitable for validation rule authoring.

## Artifacts

| Artifact | Type | Location |
|----------|------|----------|
| Required field matrix | Reference | {TASK_PATH}/reports/<YYYY-MM>/field-matrix.md |
```

## Length Guidelines

| Component | Target Length |
|-----------|---------------|
| Summary | 15-40 words |
| Completion criteria | 1-2 sentences |
| Context | 20-50 words |
| Total task | 100-250 words |

Tasks are intentionally brief. Extensive detail suggests the task scope is too large.

## Validation Checklist

- [ ] Record fields set (`ctxmod work show`): kind, title, status, parent
- [ ] Parent references valid issue identifier
- [ ] Title uses imperative verb phrase
- [ ] Summary describes contribution to parent issue
- [ ] Completion criteria is single and verifiable
- [ ] Context includes artifact references
- [ ] Dependencies stated if sequential order required
- [ ] Word count within guidelines
