# Story Work Item Schema

Stories define deliverable outcomes providing observable value. They bridge strategic epics and actionable issues. Each story articulates what value is delivered and to whom, establishes acceptance criteria, and identifies implementation issues.

## Record Fields

A story's state lives in its work-store record, never in the body: the store writes the managed fields, and `ITEM.md` carries no managed metadata (frontmatter in a body is author text the store neither reads nor splits). Create the record and set unmanaged fields at creation:

```sh
ctxmod work new story "<title>" --parent <parent id> --set priority=<critical|high|medium|low> [--set responsible=<assignee identifier>]
```

| Field | Required | Set by | Description |
|-------|----------|--------|-------------|
| id | Yes | store (`ctxmod work new`) | Ten-letter record id; legacy-style labels (S01, S02, etc.) may appear in titles but never replace the id |
| kind | Yes | store | Always `story` |
| status | Yes | `new`, `set` | Current lifecycle state (see Status Lifecycle) |
| priority | Yes | `--set priority=` | Relative importance (unmanaged) |
| created | Yes | store | Creation instant (RFC 3339 UTC) |
| parent | Conditional | `new --parent`, `set parent=` | Parent epic if applicable — the parent's record id |
| responsible | No | `--set responsible=` | Responsible party (unmanaged; the store's own `owner` field names the item an artifact was created under) |
| title | Yes | `new`, `set` | One-line title; mirrors the body's H1 |

## Required Sections

### Title

Describes the deliverable outcome from the recipient's perspective.

**Format**: Outcome-oriented statement

**Examples**:
- "Contributors Can Validate Content Before Submission"
- "Reports Include Standardized Metadata"
- "Configuration Changes Propagate Automatically"

### Summary

2-4 sentences establishing what value this story delivers and who benefits.

**Content requirements**:
- What changes for the better when complete
- Who experiences the improvement
- Observable difference from current state

Avoid implementation details—describe outcomes, not methods.

### Acceptance Criteria

Conditions that must hold true for the story to be complete.

```markdown
## Acceptance Criteria

- [ ] {First verifiable condition}
- [ ] {Second verifiable condition}
- [ ] {Third verifiable condition}
```

**Criteria characteristics**:

| Characteristic | Description |
|----------------|-------------|
| Binary | Pass or fail without degrees |
| Observable | Verifiable through examination |
| Independent | Each criterion stands alone |
| Necessary | Removing it leaves story incomplete |
| Sufficient | Meeting all criteria means story is done |

### Issues

Work items required to complete this story.

```markdown
## Issues

| Issue | Description | Type | Status |
|-------|-------------|------|--------|
| I01 | {What this issue addresses} | Enhancement | open |
| I02 | {What this issue addresses} | Task | open |
```

Issues reference their parent story through their `parent` field; `ctxmod work list --parent <story>` is authoritative for their status. Story completion requires all issues resolved and acceptance criteria verified.

## Optional Sections

### Context

Background when summary needs expansion. Include history, constraints, or stakeholder considerations.

### Dependencies

Prerequisites or related items:

```markdown
## Dependencies

| Dependency | Type | Impact |
|------------|------|--------|
| {What is needed} | Story | Blocks start |
| {What is needed} | External | Informs approach |
```

### Constraints

Technical, organizational, or temporal limitations affecting approach.

### Out of Scope

Explicitly excluded work to prevent scope creep.

### Notes

Implementation considerations without prescribing solutions.

## Value Statement

Every story must articulate value:

| Component | Question | Pattern |
|-----------|----------|---------|
| Recipient | Who benefits? | "Users", "Operators", "Downstream consumers" |
| Capability | What can they do? | "can verify", "will receive", "no longer need to" |
| Benefit | Why does it matter? | "reducing errors", "enabling automation" |

Avoid stories describing activities ("implement X") rather than outcomes ("X functions correctly").

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

A story is an item record; its body and everything it owns live in its item directory:

```
items/<YYYY-MM>/<DD-HHMM>-<id>-<slug>/   # under the store directory (`ctxmod state show`)
├── ITEM.md                              # This document
├── reports/                             # Story-scoped reports (created with --owner <id>)
│   └── <YYYY-MM>/
│       └── <DD-HHMM>-<report id>-<slug>.md
└── ...                                  # intents/, plans/, … for other owned records
```

Items never nest physically: the hierarchy is the `parent` field (a story's parent is an epic), and `ctxmod work set <id> parent=<id>` re-parents without moving a file.

## Example

```markdown
<!-- record state (S01): kind=story status=in_progress parent=<id of E01> priority=high responsible=documentation-team -->

# Contributors Can Validate Content Before Submission

## Summary

Contributors verify their content meets standards before submitting for review. Validation catches common errors early, reducing review cycles. Reviewers receive content that already meets baseline requirements.

## Acceptance Criteria

- [ ] Validation identifies missing required sections
- [ ] Validation flags style guideline violations
- [ ] Validation reports are actionable with specific locations
- [ ] Validation completes without external dependencies
- [ ] Contributors can run validation on individual files

## Issues

| Issue | Description | Type | Status |
|-------|-------------|------|--------|
| I01 | Define validation rules for required sections | Enhancement | in_progress |
| I02 | Define validation rules for style guidelines | Enhancement | open |
| I03 | Create validation report format | Enhancement | open |
| I04 | Document validation usage for contributors | Documentation | open |

## Dependencies

| Dependency | Type | Impact |
|------------|------|--------|
| Template definitions | Story S02 | Required for section validation |

## Constraints

- Validation must work offline
- No external service dependencies

## Out of Scope

- Automated fixes for violations
- Integration with external tools
- Custom rule definitions
```

## Length Guidelines

| Component | Target Length |
|-----------|---------------|
| Summary | 30-60 words |
| Acceptance criteria | 3-8 criteria |
| Issues | 2-10 issues |
| Total story | 300-600 words |

## Validation Checklist

- [ ] Record fields set (`ctxmod work show`): kind, title, status, priority
- [ ] `parent` set if the story belongs to an epic
- [ ] Title is outcome-focused from recipient perspective
- [ ] Summary articulates value and beneficiary
- [ ] Acceptance criteria are binary and observable
- [ ] Issues listed with descriptions and types
- [ ] Word count within guidelines
