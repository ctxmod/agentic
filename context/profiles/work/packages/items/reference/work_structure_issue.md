# Issue Work Item Schema

Issues define specific work units that can be assigned, tracked, and completed independently. They document problems requiring correction, enhancements adding capability, or general work requiring attention. Issues provide sufficient context to begin work and establish clear completion criteria.

## Record Fields

An issue's state lives in its work-store record, never in the body: the store writes the managed fields, and `ITEM.md` carries no managed metadata (frontmatter in a body is author text the store neither reads nor splits). Create the record and set unmanaged fields at creation:

```sh
ctxmod work new issue "<title>" --parent <parent id> --set issue_type=<bug|enhancement|refactor|documentation|research|task> --set priority=<critical|high|medium|low> [--set responsible=<assignee identifier>]
```

| Field | Required | Set by | Description |
|-------|----------|--------|-------------|
| id | Yes | store (`ctxmod work new`) | Ten-letter record id; legacy-style labels (I01, I02, etc.) may appear in titles but never replace the id |
| kind | Yes | store | Always `issue` |
| issue_type | Yes | `--set issue_type=` | Classification of issue nature (unmanaged) |
| status | Yes | `new`, `set` | Current lifecycle state (see Status Lifecycle) |
| priority | Yes | `--set priority=` | Relative importance (unmanaged) |
| created | Yes | store | Creation instant (RFC 3339 UTC) |
| parent | Conditional | `new --parent`, `set parent=` | Parent story if applicable — the parent's record id |
| responsible | No | `--set responsible=` | Responsible party (unmanaged; the store's own `owner` field names the item an artifact was created under) |
| title | Yes | `new`, `set` | One-line title; mirrors the body's H1 |

## Issue Type Classification

| Type | Description |
|------|-------------|
| bug | Incorrect behavior requiring correction |
| enhancement | New capability or improvement |
| refactor | Structural change without behavior change |
| documentation | Content creation or modification |
| research | Investigation producing findings |
| task | General work not fitting other categories |

## Required Sections

### Title

Concise description of the work or problem.

**Format**: Descriptive statement specific enough to distinguish from other issues

**Examples**:
- "Validation Rejects Valid Nested Structures"
- "Add Required Section Detection"
- "Document Configuration Precedence Rules"

### Summary

2-4 sentences describing the issue.

**For bugs**: Describe current versus expected behavior
**For enhancements**: Describe the capability gap
**For research**: Describe the question and expected output

### Completion Criteria

Conditions that must hold for the issue to be resolved.

```markdown
## Completion Criteria

- [ ] {First verifiable condition}
- [ ] {Second verifiable condition}
```

**Criteria characteristics**:

| Characteristic | Description |
|----------------|-------------|
| Binary | Complete or incomplete |
| Observable | Verifiable through examination |
| Scoped | Bounded to this issue |
| Necessary | Required for resolution |

### Context

Information enabling work to begin.

```markdown
## Context

**Relevant artifacts**:
- {Path or reference to relevant item}
- {Path or reference to relevant item}

**Related issues**: {Dependencies or related work}

**Constraints**: {Limitations affecting approach}
```

## Optional Sections

### Reproduction Steps

For bugs, steps to observe the problem:

```markdown
## Reproduction Steps

1. {First step}
2. {Second step}
3. {Observe: description of problem}
```

### Expected Behavior

For bugs, what should happen instead:

```markdown
## Expected Behavior

{Description of correct behavior}
```

### Current Behavior

For bugs, explicit statement of incorrect behavior when summary is insufficient.

### Tasks

For complex issues, granular work items:

```markdown
## Tasks

| Task | Description | Status |
|------|-------------|--------|
| T01 | {Specific activity} | Pending |
| T02 | {Specific activity} | Pending |
```

Tasks are optional. Use when an issue requires progress tracking at finer granularity.

### Notes

Additional context without prescribing solutions.

### Approach

Suggested direction when the path forward is unclear. Avoid over-specifying implementation.

## Type-Specific Structure

### Bug Issues

| Section | Content |
|---------|---------|
| Summary | Current versus expected behavior |
| Reproduction Steps | Steps to observe problem |
| Expected Behavior | What should happen |
| Impact | Severity and affected scope |

### Enhancement Issues

| Section | Content |
|---------|---------|
| Summary | Capability gap and desired outcome |
| Value | Why this enhancement matters |
| Scope | Boundaries of the change |

### Research Issues

| Section | Content |
|---------|---------|
| Summary | Question requiring investigation |
| Context | Why this question matters |
| Deliverable | Expected output format |
| Scope | Investigation boundaries |

## Status Lifecycle

Statuses are the store's item vocabulary (`ctxmod work set <id> status=<status>`); the store accepts any order, so the transitions below are this module's doctrine.

| Status | Meaning | Transitions To |
|--------|---------|----------------|
| `open` (draft) | Being defined | in_progress, cancelled |
| `in_progress` (active) | Ready for work | done, blocked, cancelled |
| `blocked` | Cannot proceed | in_progress, cancelled |
| `done` (completed) | All criteria met | (terminal) |
| `cancelled` | No longer relevant | (terminal) |

## Path Convention

A issue is an item record; its body and everything it owns live in its item directory:

```
items/<YYYY-MM>/<DD-HHMM>-<id>-<slug>/   # under the store directory (`ctxmod state show`)
├── ITEM.md                              # This document
├── reports/                             # Issue-scoped reports (created with --owner <id>)
│   └── <YYYY-MM>/
│       └── <DD-HHMM>-<report id>-<slug>.md
└── ...                                  # intents/, plans/, … for other owned records
```

Items never nest physically: the hierarchy is the `parent` field (a issue's parent is a story or epic), and `ctxmod work set <id> parent=<id>` re-parents without moving a file.

## Example

```markdown
<!-- record state (I01): kind=issue status=in_progress parent=<id of S01> priority=high issue_type=enhancement responsible=validation-team -->

# Add Required Section Detection

## Summary

Validation does not currently check for required sections. Contributors submit content missing mandatory elements, causing review rejections. Detection of missing sections enables early correction.

## Completion Criteria

- [ ] Validation identifies all required sections per artifact type
- [ ] Missing sections reported with expected location
- [ ] Validation distinguishes between missing and empty sections
- [ ] Report format matches validation schema

## Context

**Relevant artifacts**:
- Template definitions: `reference/writing/schemas/`
- Current validation: `reference/procedures/validation/`

**Related issues**: I02 depends on section detection being complete

**Constraints**: Must work with existing report format

## Tasks

| Task | Description | Status |
|------|-------------|--------|
| T01 | Define required sections per artifact type | Pending |
| T02 | Implement section presence checking | Pending |
| T03 | Format missing section reports | Pending |

## Notes

Consider whether section order should be validated alongside presence.
```

## Length Guidelines

| Component | Target Length |
|-----------|---------------|
| Summary | 30-60 words |
| Completion criteria | 2-5 criteria |
| Context | 30-80 words |
| Total issue | 200-500 words |

## Validation Checklist

- [ ] Frontmatter includes id, type, issue-type, status, priority, created
- [ ] Parent field present if issue belongs to story
- [ ] Title is specific and descriptive
- [ ] Summary describes problem or capability gap
- [ ] Completion criteria are binary and scoped
- [ ] Context provides artifact references
- [ ] Issue-type matches content structure
- [ ] Word count within guidelines
