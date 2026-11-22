# Implementation Report Schema

Implementation reports document changes made and their verification results. They record what was modified, how it was changed, and whether the changes achieved their intended purpose. Implementation provides an audit trail for modifications and enables review of completed work.

## Frontmatter

```yaml
---
type: implementation
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
scope: "{implementation scope description}"
status: "{complete | partial | failed}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `implementation` |
| date | Yes | Report creation date |
| time | Yes | Report creation time |
| source | Yes | Work item context or "general" |
| objective | Yes | Objective that produced this report |
| scope | Yes | What was implemented |
| status | Yes | Overall result: complete, partial, or failed |

## Required Sections

### Summary

2-4 sentences providing overview. State:

- Changes completed
- Verification status
- Any outstanding items

### Context

Task or objective driving the implementation:

```markdown
## Context

**Objective**: {what this implementation achieves}

**Triggering Report**: {analysis or validation report if applicable}

**Requirements**: {reference to requirements or acceptance criteria}
```

### Changes Made

File-by-file or item-by-item change descriptions:

```markdown
### {File or Item Path}

- **Change Type**: Create | Edit | Delete | Rename | Move
- **Description**: {what changed}
- **Location**: {line range or section for edits}
- **Rationale**: {why this change was made}
```

Group related changes logically. For renames or moves, include both original and new paths.

### Verification Results

Outcomes confirming changes achieved purpose:

```markdown
## Verification Results

| Verification | Result | Notes |
|--------------|--------|-------|
| {verification type} | Pass | {additional context} |
| {verification type} | Fail | {failure details} |
```

Include specific verification methods: testing, validation, review, or confirmation.

## Optional Sections

### Dependencies

Changes that depend on or are depended upon by other work:

```markdown
## Dependencies

**Requires**: {items that must exist for these changes to work}

**Enables**: {items that can now proceed after these changes}
```

### Follow-up Items

Remaining work or concerns discovered during implementation:

```markdown
### {Follow-up Item}

- **Type**: Bug | Enhancement | Documentation | Investigation
- **Priority**: High | Medium | Low
- **Description**: {what needs attention}
```

### Rollback Instructions

Steps to reverse changes if needed:

```markdown
## Rollback Instructions

1. {Step to undo change}
2. {Step to restore previous state}
3. {Verification of rollback}
```

### Related Reports

Links to triggering or subsequent reports:

- Triggering: `{path to analysis or validation report}`
- Subsequent: `{path to verification or summary report}`

## Change Types

| Type | Description | Documentation |
|------|-------------|---------------|
| Create | New file or item added | Full path, purpose |
| Edit | Existing content modified | Path, line range, before/after summary |
| Delete | File or item removed | Path, reason for removal |
| Rename | Name changed without content change | Original and new paths |
| Move | Location changed without content change | Original and new paths |

## Example

```markdown
---
type: implementation
date: {date}
time: 10-15
source: general
objective: implementation
scope: Configuration naming standardization
status: complete
---

# Configuration Naming Implementation Report

## Summary

Renamed 3 configuration files to comply with naming convention. All changes verified against validation requirements. No follow-up items identified.

## Context

**Objective**: Standardize configuration file names per REQ-CONFIG-001

**Triggering Report**: {time_2}-validation-naming.md (3 violations)

**Requirements**: Naming Convention Standard v2.1 (conventions.md:15-45)

## Changes Made

### config/database.prod.yaml → config/database.yaml.prod

- **Change Type**: Rename
- **Description**: Changed environment suffix position
- **Rationale**: Align with `.{format}.{env}` pattern per REQ-CONFIG-001

### config/settings.yaml → config/app-settings.yaml

- **Change Type**: Rename
- **Description**: Added purpose prefix
- **Rationale**: Provide clarity per REQ-CONFIG-001 § Purpose Identification

### config/db.yaml → config/database.yaml

- **Change Type**: Rename
- **Description**: Expanded abbreviation to full word
- **Rationale**: Improve clarity per REQ-CONFIG-001 § Naming Clarity

## Verification Results

| Verification | Result | Notes |
|--------------|--------|-------|
| Naming validation | Pass | All 12 files now comply |
| Reference integrity | Pass | No broken references detected |
| Load verification | Pass | Configuration loads correctly |

## Dependencies

**Requires**: None

**Enables**: Configuration module can proceed with standardized names

## Related Reports

- Triggering: `reports/{date}/{time_2}-validation-naming.md`
```

## Length Guidelines

| Scope | Target Length |
|-------|---------------|
| Single change | 200-400 words |
| Multiple related changes | 400-700 words |
| Major implementation | 700-1000 words |

## Validation Checklist

- [ ] Frontmatter includes all required fields with status
- [ ] Summary states changes and verification outcome
- [ ] Context documents objective and triggering work
- [ ] Each change includes type, description, and rationale
- [ ] Verification results confirm change effectiveness
- [ ] Change types used consistently
- [ ] Follow-up items captured if any
- [ ] Word count within guidelines
