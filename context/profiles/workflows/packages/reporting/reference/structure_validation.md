# Validation Report Schema

Validation reports document compliance status and categorize findings. They verify that artifacts meet defined requirements, conventions, or expectations. Validation answers "does it meet requirements?"—distinct from discovery (what exists) and analysis (what it means).

## Frontmatter

```yaml
---
type: validation
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
scope: "{validation scope description}"
status: "{pass | fail | partial}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `validation` |
| date | Yes | Report creation date |
| time | Yes | Report creation time |
| source | Yes | Work item context or "general" |
| objective | Yes | Objective that produced this report |
| scope | Yes | What was validated |
| status | Yes | Overall result: pass, fail, or partial |

## Required Sections

### Summary

2-4 sentences providing compliance overview. State:

- Artifacts validated
- Overall pass/fail status
- Violation counts by severity
- Critical blockers (if any)

Enable quick assessment without reading details.

### Requirements

Source of validation criteria:

```markdown
## Requirements

Validation against:

- **Primary**: {requirement source with reference}
- **Secondary**: {additional standards if applicable}

Criteria version: {version or date if applicable}
```

### Compliance Status

High-level assessment with metrics:

```markdown
## Compliance Status

| Metric | Value |
|--------|-------|
| Total items validated | 15 |
| Passed | 12 (80%) |
| Failed | 3 (20%) |

**By Severity**:
- Critical: 0
- High: 1
- Medium: 2
- Low: 0
- Info: 3
```

### Violations

Categorized findings. Each violation includes complete documentation:

```markdown
### {Violation Title}

- **Location**: {path, line range, or identifier}
- **Requirement**: {reference to violated criteria}
- **Observed**: {what was found}
- **Expected**: {what should exist}
- **Severity**: Critical | High | Medium | Low | Info
- **Remediation**: {how to correct}
```

Group violations by type, artifact, or severity depending on report purpose.

## Optional Sections

### Passed Items

When tracking compliance is valuable:

```markdown
## Passed Items

| Item | Requirement | Status |
|------|-------------|--------|
| Naming convention | REQ-001 | Pass |
| Required fields | REQ-002 | Pass |
```

### Exceptions

Documented deviations that are intentional:

```markdown
## Exceptions

### {Exception Description}

- **Location**: {affected item}
- **Deviation**: {how it differs from requirement}
- **Justification**: {why this is acceptable}
- **Approved by**: {authorization reference}
```

### Recommendations

Priority-ordered corrections when remediation is complex:

```markdown
## Recommendations

1. Address Critical violations before proceeding
2. Batch Medium violations by affected area
3. Schedule Low violations for maintenance cycle
```

### Methodology

Validation approach when non-obvious. Document tools, sampling strategy, or coverage approach.

## Severity Classification

| Severity | Impact | Action |
|----------|--------|--------|
| Critical | Blocks use, causes failure, security risk | Immediate remediation required |
| High | Significant impact, major deviation | Remediate before proceeding |
| Medium | Noticeable quality issue, moderate deviation | Remediate when practical |
| Low | Minor issue, slight deviation | Remediate opportunistically |
| Info | Observation without violation | No action required |

Classify based on impact to consumers, not effort to remediate.

## Violation Documentation

Each violation requires:

| Field | Content |
|-------|---------|
| Location | Path with line range or identifier |
| Requirement | Reference to violated criteria |
| Observed | What was found (factual) |
| Expected | What should exist (from requirement) |
| Severity | Classification level |
| Remediation | How to correct the violation |

Incomplete violation entries reduce report actionability.

## Example

```markdown
---
type: validation
date: {date}
time: {time}
source: general
objective: validation
scope: Configuration naming conventions
status: fail
---

# Configuration Naming Validation Report

## Summary

Validated 12 configuration files against naming convention REQ-CONFIG-001. 9 files pass (75%), 3 files fail. One High severity violation requires attention before deployment.

## Requirements

Validation against:

- **Primary**: Naming Convention Standard v2.1 (conventions.md:15-45)
- **Secondary**: Directory Structure Guidelines (structure.md § Configuration)

## Compliance Status

| Metric | Value |
|--------|-------|
| Total files validated | 12 |
| Passed | 9 (75%) |
| Failed | 3 (25%) |

**By Severity**:
- Critical: 0
- High: 1
- Medium: 2
- Low: 0

## Violations

### Inconsistent Environment Suffix

- **Location**: `config/database.prod.yaml`
- **Requirement**: REQ-CONFIG-001 § Naming Pattern
- **Observed**: Uses `.prod.` infix pattern
- **Expected**: Uses `.yaml.prod` suffix pattern
- **Severity**: High
- **Remediation**: Rename to `database.yaml.prod`

### Missing Purpose Prefix

- **Location**: `config/settings.yaml`
- **Requirement**: REQ-CONFIG-001 § Purpose Identification
- **Observed**: Generic name without purpose identifier
- **Expected**: Specific purpose prefix (e.g., `app-settings.yaml`)
- **Severity**: Medium
- **Remediation**: Rename with descriptive purpose prefix

### Abbreviated Name

- **Location**: `config/db.yaml`
- **Requirement**: REQ-CONFIG-001 § Clarity
- **Observed**: Abbreviated `db` instead of full word
- **Expected**: Full word `database` for clarity
- **Severity**: Medium
- **Remediation**: Rename to `database.yaml`

## Metadata

- **Files validated**: 12 files in `config/` directory
- **Method**: Pattern matching against naming standard regex
- **Limitations**: Runtime-generated names not validated
```

## Length Guidelines

| Scope | Target Length |
|-------|---------------|
| Single artifact | 300-600 words |
| Category or domain | 500-1000 words |
| Cross-domain | 800-1500 words |

Violation-heavy validations require more detail. Clean validations can be brief.

## Validation Checklist

- [ ] Frontmatter includes all required fields with overall status
- [ ] Summary states pass/fail status with violation counts
- [ ] Requirements section documents validation criteria
- [ ] Compliance status provides metrics overview
- [ ] Each violation includes all six required fields
- [ ] Severity classification applied consistently
- [ ] Remediation guidance is actionable
- [ ] Word count within guidelines
