# Remediation Report Schema

Remediation reports record corrections applied in response to identified issues. They document which issues were addressed, how they were resolved, and the verification of successful correction. Remediation closes the loop between validation and implementation by confirming issue resolution.

## Frontmatter

```yaml
---
type: remediation
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
scope: "{remediation scope description}"
input_validation: "{path to validation report}"
status: "{complete | partial | failed}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `remediation` |
| date | Yes | Report creation date |
| time | Yes | Report creation time |
| source | Yes | Work item context or "general" |
| objective | Yes | Objective that produced this report |
| scope | Yes | What was remediated |
| input_validation | Yes | Path to triggering validation report |
| status | Yes | Overall result: complete, partial, or failed |

## Required Sections

### Summary

2-4 sentences providing overview. State:

- Issues addressed count
- Resolution status (all resolved, partial, or none)
- Re-validation outcome

### Input Validation

Reference to the validation report driving remediation:

```markdown
## Input Validation

**Report**: {path to validation report}

**Issues Identified**: {count by severity}

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 1 |
| Medium | 2 |
| Low | 0 |
```

### Issues Resolved

Per-issue resolution documentation:

```markdown
### {Issue Title from Validation}

**Original Issue**:
- Location: {original location}
- Severity: {original severity}
- Observed: {what was wrong}

**Resolution**:
- Action: {what was done}
- New Location: {if changed}
- Verification: Pass | Fail
```

### Remaining Issues

Issues not resolved (required when status is partial or failed):

```markdown
### {Unresolved Issue Title}

- **Original Location**: {where issue exists}
- **Severity**: {issue severity}
- **Reason Not Resolved**: {why remediation did not occur}
- **Recommended Action**: {suggested next step}
```

### Verification

Re-validation results confirming resolution:

```markdown
## Verification

**Method**: {how resolution was verified}

**Results**:

| Issue | Resolution | Verification |
|-------|------------|--------------|
| Issue 1 | Fixed | Pass |
| Issue 2 | Fixed | Pass |
| Issue 3 | Deferred | N/A |

**Post-Remediation Status**: {overall compliance after remediation}
```

## Optional Sections

### Root Cause Analysis

When issues share common causes:

```markdown
## Root Cause Analysis

### {Root Cause Title}

**Affected Issues**: {list of issues sharing this cause}

**Cause**: {underlying reason for issues}

**Systemic Fix**: {action to prevent recurrence}
```

### Prevention Recommendations

Steps to avoid similar issues:

```markdown
## Prevention Recommendations

1. {Recommendation for avoiding recurrence}
2. {Process or tooling improvement}
```

### Related Reports

Links to subsequent work:

- Implementation: `{path to implementation report if separate}`
- Follow-up Validation: `{path to re-validation report}`

## Resolution Status

| Status | Definition |
|--------|------------|
| Fixed | Issue corrected and verified |
| Deferred | Issue acknowledged but not addressed now |
| Won't Fix | Issue intentionally left unresolved |
| Cannot Fix | Issue requires external action |
| Partially Fixed | Issue partially addressed |

Document rationale for any status other than Fixed.

## Example

```markdown
---
type: remediation
date: {date}
time: 11-30
source: general
objective: remediation
scope: Configuration naming violations
input_validation: reports/{date}/{time_2}-validation-naming.md
status: complete
---

# Configuration Naming Remediation Report

## Summary

Addressed 3 naming violations from validation report. All issues resolved and verified. Post-remediation validation shows 100% compliance.

## Input Validation

**Report**: reports/{date}/{time_2}-validation-naming.md

**Issues Identified**:

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 1 |
| Medium | 2 |
| Low | 0 |

## Issues Resolved

### Inconsistent Environment Suffix

**Original Issue**:
- Location: `config/database.prod.yaml`
- Severity: High
- Observed: Uses `.prod.` infix pattern

**Resolution**:
- Action: Renamed file to standard suffix pattern
- New Location: `config/database.yaml.prod`
- Verification: Pass

### Missing Purpose Prefix

**Original Issue**:
- Location: `config/settings.yaml`
- Severity: Medium
- Observed: Generic name without purpose identifier

**Resolution**:
- Action: Added descriptive purpose prefix
- New Location: `config/app-settings.yaml`
- Verification: Pass

### Abbreviated Name

**Original Issue**:
- Location: `config/db.yaml`
- Severity: Medium
- Observed: Abbreviated `db` instead of full word

**Resolution**:
- Action: Expanded abbreviation
- New Location: `config/database.yaml`
- Verification: Pass

## Verification

**Method**: Re-ran naming validation against all configuration files

**Results**:

| Issue | Resolution | Verification |
|-------|------------|--------------|
| Environment suffix | Fixed | Pass |
| Purpose prefix | Fixed | Pass |
| Abbreviated name | Fixed | Pass |

**Post-Remediation Status**: 12 of 12 files compliant (100%)

## Prevention Recommendations

1. Add naming validation to pre-commit checks
2. Document naming convention in contributor guidelines
```

## Length Guidelines

| Scope | Target Length |
|-------|---------------|
| 1-3 issues | 300-500 words |
| 4-6 issues | 500-800 words |
| 7+ issues | 800-1200 words |

## Validation Checklist

- [ ] Frontmatter includes all required fields with input validation path
- [ ] Summary states issue count and resolution status
- [ ] Input validation section references source report
- [ ] Each resolved issue documents original state and resolution
- [ ] Remaining issues section present if status is partial or failed
- [ ] Verification section confirms resolution with method
- [ ] Resolution status used consistently
- [ ] Word count within guidelines
