# Correction Planning Procedure

Convert issue analysis into an executable correction plan with proper ordering, dependency management, and verification checkpoints.

## Prerequisites

- Issue analysis completed with root causes and complexity ratings
- Access to affected artifacts for scope assessment
- Understanding of artifact relationships and dependencies
- Recovery mechanism available (backup, version control)

## Steps

### 1. Group Corrections by Root Cause

Organize corrections around root causes rather than individual findings:

```markdown
## Correction Groups

### CG1: Template Drift (3 findings)
Target: F1, F4, F7
Approach: Add missing sections to affected files
Files: doc.md, guide.md, spec.md

### CG2: Broken References (2 findings)
Target: F2, F5
Approach: Update paths to new locations
Files: config.yaml, manifest.json
```

Single-finding root causes become individual correction items.

### 2. Map Correction Dependencies

Identify ordering constraints between corrections:

| Dependency Type | Description | Example |
|-----------------|-------------|---------|
| **Data** | Correction produces content another needs | Add schema → use schema |
| **Reference** | Target must exist before reference valid | Create file → reference file |
| **Consistency** | Changes must coordinate | Rename A → update references to A |
| **Logical** | Semantic ordering requirement | Define term → use term |

Build dependency graph:

```
CG3 (create schema) → CG2 (update references) → CG4 (validate consistency)
CG1 (structural fixes) [independent]
```

### 3. Determine Execution Order

Derive sequence from dependencies using topological ordering:

1. **Independent corrections first**: No incoming dependencies
2. **Dependent corrections follow**: After prerequisites complete
3. **Parallel-safe corrections**: Independent items can execute simultaneously

```markdown
## Execution Sequence

### Phase 1 (parallel-safe)
- CG1: Add missing sections
- CG3: Create schema file

### Phase 2 (depends on Phase 1)
- CG2: Update reference paths

### Phase 3 (depends on Phase 2)
- CG4: Consistency alignment
```

### 4. Define Verification Checkpoints

Establish verification points between phases:

| Checkpoint | After | Verification | Pass Criteria |
|------------|-------|--------------|---------------|
| CP0 | Baseline | Current state valid | Artifacts accessible |
| CP1 | Phase 1 | Structural checks | New sections present, schema valid |
| CP2 | Phase 2 | Reference checks | All paths resolve |
| CP3 | Phase 3 | Consistency checks | Terminology aligned |

Specify verification method for each checkpoint:

- **Read verification**: Artifact contains expected content
- **Parse verification**: Artifact structurally valid
- **Reference verification**: Links and paths resolve
- **Validation re-run**: Subset of original validation

### 5. Specify Correction Actions

Detail each correction with precise instructions:

```markdown
### CG1-C1: Add Configuration Section to doc.md

**Target**: doc.md
**Location**: After "## Overview" section
**Action**: Insert section
**Content**:
[Template or specification of content to add]

**Verification**: Section header present, content matches template
```

For complex corrections, include decision criteria:

```markdown
### CG2-C1: Update schema reference

**Target**: config.yaml:42
**Current**: schema: "../old/schema.json"
**New**: schema: "./schemas/main.json"
**Decision**: If target path doesn't exist, create placeholder and flag for review
```

### 6. Document Rollback Strategy

For each checkpoint, specify recovery approach:

| From | To | Rollback Action |
|------|-----|-----------------|
| CP1 | CP0 | Revert added sections |
| CP2 | CP1 | Restore original paths |
| CP3 | CP2 | Revert terminology changes |

Note cascading effects: rolling back may invalidate subsequent work.

### 7. Produce Correction Plan

Compile into executable document:

```markdown
## Correction Plan

### Summary
- Correction groups: [N]
- Total corrections: [N]
- Phases: [N]
- Estimated complexity: [rating]

### Dependencies
[Dependency graph or list]

### Execution Sequence
[Ordered phases with corrections]

### Verification Checkpoints
[Checkpoint table with verification methods]

### Rollback Strategy
[Rollback table]

### Outstanding Questions
[Any unresolved issues requiring decision]
```

## Decision Points

**Circular dependencies**: Break cycle by identifying minimum edit that satisfies both constraints, or decompose correction into smaller steps.

**Complexity exceeds capacity**: Split plan into multiple sessions. Define stable intermediate state as completion point for first session.

**Ambiguous correction approach**: Document alternatives with trade-offs. Select approach or flag for decision before execution.

**Findings addressed by same correction**: Map multiple findings to single correction. Track which findings resolve upon completion.

## Success Criteria

- All findings from analysis addressed by corrections
- Dependencies mapped without cycles
- Execution order derived from dependencies
- Verification checkpoints defined with pass criteria
- Rollback strategy documented for each checkpoint
- Outstanding questions explicitly listed

## Common Issues

**Plan too granular**: Excessive detail slows execution. Balance specificity with practical step size.

**Missing dependencies**: Correction fails because prerequisite not identified. Trace references before finalizing order.

**Verification gaps**: Checkpoint passes but issue persists. Align verification with original validation criteria.

**Scope creep**: Plan expands beyond original findings. Constrain to analyzed issues unless new critical findings emerge.

**Optimistic complexity**: Plan assumes corrections succeed. Include contingency for common failure modes.
