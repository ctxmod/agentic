# Epic Work Item Schema

Epics define major initiatives spanning multiple stories. They establish strategic scope, articulate value, and provide measurable success criteria. Epics sit at the top of the work item hierarchy, containing stories that decompose the initiative into deliverable units.

## Record Fields

An epic's state lives in its work-store record, never in the body: the store writes the managed fields, and `ITEM.md` carries no managed metadata (frontmatter in a body is author text the store neither reads nor splits). Create the record and set unmanaged fields at creation:

```sh
ctxmod work new epic "<title>" --set priority=<critical|high|medium|low> [--set responsible=<assignee identifier>]
```

| Field | Required | Set by | Description |
|-------|----------|--------|-------------|
| id | Yes | store (`ctxmod work new`) | Ten-letter record id; legacy-style labels (E01, E02, etc.) may appear in titles but never replace the id |
| kind | Yes | store | Always `epic` |
| status | Yes | `new`, `set` | Current lifecycle state (see Status Lifecycle) |
| priority | Yes | `--set priority=` | Relative importance (unmanaged) |
| created | Yes | store | Creation instant (RFC 3339 UTC) |
| responsible | No | `--set responsible=` | Responsible party (unmanaged; the store's own `owner` field names the item an artifact was created under) |
| title | Yes | `new`, `set` | One-line title; mirrors the body's H1 |

## Required Sections

### Title

Action-oriented phrase describing the initiative outcome.

**Format**: Imperative or outcome-focused statement

**Examples**:
- "Establish Configuration Management System"
- "Migrate Legacy Data to New Format"
- "Implement Quality Assurance Framework"

### Summary

2-4 sentences establishing context and strategic value.

**Content requirements**:
- Why this initiative matters
- Who benefits from completion
- What changes when complete

Avoid implementation details—summaries describe outcomes, not methods.

### Scope

Explicit boundaries defining inclusion and exclusion.

```markdown
## Scope

### Included

- {What belongs to this initiative}
- {Affected domains or areas}
- {Deliverable categories}

### Excluded

- {What remains outside scope}
- {Adjacent work handled separately}
- {Future phases or extensions}
```

Explicit exclusions prevent scope creep and clarify boundaries for story creation.

### Success Criteria

Measurable outcomes indicating epic completion.

```markdown
## Success Criteria

| Criterion | Verification Method |
|-----------|---------------------|
| {Measurable condition} | {How to verify} |
| {Measurable condition} | {How to verify} |
```

**Criteria characteristics**:
- Verifiable without subjective judgment
- Each maps to one or more stories
- Collectively define "done" for the initiative

### Stories

List of stories comprising this epic.

```markdown
## Stories

| Story | Description | Status |
|-------|-------------|--------|
| S01 | {What this story delivers} | Pending |
| S02 | {What this story delivers} | Pending |
```

Stories reference their parent epic. Epic completion requires all stories resolved or explicitly descoped.

## Optional Sections

### Background

Extended context when summary needs expansion. Include history, constraints, or stakeholder considerations that affect scope.

### Dependencies

External dependencies affecting the initiative:

```markdown
## Dependencies

| Dependency | Type | Impact |
|------------|------|--------|
| {What is needed} | External | Blocks stories S01, S02 |
| {What is needed} | Internal | Informs scope decisions |
```

### Risks

Known risks affecting epic success:

```markdown
### {Risk Title}

- **Likelihood**: High | Medium | Low
- **Impact**: High | Medium | Low
- **Mitigation**: {Current approach}
```

### Timeline Considerations

Sequencing constraints, external deadlines, or coordination requirements without time estimates.

### Stakeholders

Affected parties and their interests when multiple stakeholders exist.

### Assumptions

Conditions presumed true that would alter scope if false.

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

A epic is an item record; its body and everything it owns live in its item directory:

```
items/<YYYY-MM>/<DD-HHMM>-<id>-<slug>/   # under the store directory (`ctxmod state show`)
├── ITEM.md                              # This document
├── reports/                             # Epic-scoped reports (created with --owner <id>)
│   └── <YYYY-MM>/
│       └── <DD-HHMM>-<report id>-<slug>.md
└── ...                                  # intents/, plans/, … for other owned records
```

Items never nest physically: the hierarchy is the `parent` field, and `ctxmod work set <id> parent=<id>` re-parents without moving a file.

## Example

```markdown
<!-- record state (E01): kind=epic status=in_progress priority=high responsible=planning-team -->

# Establish Documentation Standards

## Summary

Create consistent documentation standards across all project artifacts. Contributors will produce uniform content, and consumers will find information faster. Completion enables reliable automated processing and reduces onboarding time.

## Scope

### Included

- Template definitions for common artifact types
- Style guidelines for all written content
- Validation criteria for compliance checking
- Reference examples demonstrating correct usage

### Excluded

- Automated enforcement tooling (future phase)
- Migration of existing content (separate initiative)
- External publication workflows

## Success Criteria

| Criterion | Verification Method |
|-----------|---------------------|
| All artifact types have defined templates | Template inventory review |
| Style guidelines cover common scenarios | Guideline completeness assessment |
| Validation criteria are machine-checkable | Validation rule execution |
| Reference examples pass validation | Example validation run |

## Stories

| Story | Description | Status |
|-------|-------------|--------|
| S01 | Define core templates for primary artifact types | Active |
| S02 | Establish style guidelines for written content | Pending |
| S03 | Create validation criteria for compliance checking | Pending |
| S04 | Produce reference examples for each template | Pending |

## Dependencies

| Dependency | Type | Impact |
|------------|------|--------|
| Artifact type inventory | Internal | Required before template definition |

## Risks

### Adoption Resistance

- **Likelihood**: Medium
- **Impact**: High
- **Mitigation**: Involve contributors in guideline development

## Timeline Considerations

S01 must complete before S03 can begin. S02 and S03 can proceed in parallel once S01 completes.
```

## Length Guidelines

| Component | Target Length |
|-----------|---------------|
| Summary | 40-80 words |
| Scope section | 60-150 words |
| Success criteria | 3-7 criteria |
| Stories | 3-10 stories |
| Total epic | 400-800 words |

## Validation Checklist

- [ ] Frontmatter includes id, type, status, priority, created
- [ ] Title is outcome-focused
- [ ] Summary establishes strategic value
- [ ] Scope includes both inclusions and exclusions
- [ ] Success criteria are measurable and verifiable
- [ ] Stories listed with descriptions
- [ ] Word count within guidelines
