# Issue Analysis Procedure

Transform validation findings into actionable understanding by categorizing issues, identifying root causes, and assessing correction complexity.

## Prerequisites

- Validation report available with findings, severity levels, and locations
- Access to source artifacts referenced in findings
- Understanding of validation criteria that generated findings

## Steps

### 1. Inventory Findings

Extract findings from validation report into structured format:

| ID | Finding | Location | Severity | Category |
|----|---------|----------|----------|----------|
| F1 | Missing required section | doc.md:15 | Error | Structural |
| F2 | Broken reference | config.yaml:42 | Error | Reference |
| F3 | Inconsistent terminology | report.md:8-12 | Warning | Consistency |

Count totals by severity: errors, warnings, info items.

### 2. Categorize Findings

Assign each finding to a category based on its nature:

| Category | Characteristics | Typical Cause |
|----------|-----------------|---------------|
| **Structural** | Format violations, missing sections, malformed elements | Template drift, incomplete creation |
| **Content** | Accuracy errors, clarity issues, standard violations | Author oversight, unclear requirements |
| **Reference** | Broken links, invalid paths, orphaned references | Path changes, deleted targets |
| **Consistency** | Cross-artifact conflicts, terminology variance | Independent edits, missing coordination |
| **Completeness** | Coverage gaps, missing required elements | Scope misunderstanding, incomplete work |

### 3. Identify Root Causes

For each finding, trace backward to the originating cause:

1. **Examine the artifact** at the finding location
2. **Compare against requirements** to understand the gap
3. **Check related artifacts** for contributing factors
4. **Distinguish symptoms from causes**: Multiple findings may share one root cause

Group findings by shared root cause:

```
Root Cause: Section template updated without propagating to existing files
  - F1: Missing required section (doc.md)
  - F4: Missing required section (guide.md)
  - F7: Missing required section (spec.md)
```

### 4. Assess Correction Complexity

Rate each finding or root cause by correction difficulty:

| Complexity | Characteristics | Examples |
|------------|-----------------|----------|
| **Trivial** | Single edit, obvious fix, no dependencies | Typo, formatting, single line change |
| **Simple** | Multiple edits, clear approach, isolated scope | Add missing section, fix reference path |
| **Moderate** | Coordinated changes, requires context, affects dependencies | Terminology update across files, structural reorganization |
| **Complex** | Investigation required, approach unclear, significant impact | Consistency resolution, major content rewrite |

### 5. Document Analysis

Produce analysis summary:

```markdown
## Issue Analysis Summary

### Totals
- Errors: [N]
- Warnings: [N]
- Info: [N]

### Root Causes
1. [Root cause]: [N] findings, [complexity]
2. [Root cause]: [N] findings, [complexity]

### Category Distribution
| Category | Count | Primary Root Cause |
|----------|-------|-------------------|
| Structural | 3 | Template drift |
| Reference | 2 | Path changes |

### Findings Requiring Investigation
- [Finding]: [Reason investigation needed]
```

## Decision Points

**Multiple possible root causes**: List all candidates with supporting evidence. Correction planning can proceed with uncertainty documented.

**Findings resist categorization**: Create "Other" category temporarily. Investigate further during correction planning.

**Root cause spans validation boundaries**: Note cross-validation dependencies. May require re-validation after correction.

**Trivial findings in bulk**: Batch similar trivial findings for efficient correction. Avoid individual analysis overhead.

## Success Criteria

- All findings inventoried with severity and location
- Every finding assigned to category
- Root causes identified with affected finding groups
- Complexity assessed for each root cause
- Analysis documented with totals and distributions

## Common Issues

**Symptom-level analysis**: Treating each finding independently misses shared root causes. Group findings before determining causes.

**Over-attribution**: Assigning unrelated findings to single root cause. Each grouping must have clear causal connection.

**Complexity underestimation**: Simple-looking findings may have hidden dependencies. Verify scope before rating trivial or simple.

**Validation report staleness**: Findings may no longer apply if artifacts changed. Verify current state before deep analysis.
