# Correction Execution Procedure

Execute correction plan systematically, verifying each checkpoint before proceeding and maintaining rollback capability throughout.

## Prerequisites

- Correction plan completed with execution sequence and checkpoints
- Affected artifacts accessible and in known baseline state
- Recovery mechanism verified (backup current, version control clean)
- Verification methods available for each checkpoint

## Steps

### 1. Validate Baseline State

Before any modification, confirm starting conditions:

```markdown
## Baseline Verification (CP0)
- Target artifacts exist: [list verified]
- Artifacts match expected state: [confirmed/discrepancies]
- Recovery mechanism ready: [backup location or commit hash]
- Timestamp: [when verified]
```

**Stop if**: Artifacts changed since analysis, baseline does not match plan assumptions.

### 2. Execute Corrections by Phase

Process each phase in planned order:

#### Per-Correction Workflow

1. **Announce**: Log correction ID and target
2. **Read current state**: Verify target location and content
3. **Apply modification**: Execute planned change
4. **Verify immediate effect**: Confirm change applied correctly
5. **Record result**: Log success or failure with details

```markdown
### Correction CG1-C1
Target: doc.md
Action: Insert section after "## Overview"
Result: SUCCESS - Section added at line 15
```

For failed corrections:
```markdown
### Correction CG2-C1
Target: config.yaml:42
Action: Update path reference
Result: FAILED - Target line contains different content than expected
Observed: schema: "../../legacy/schema.json"
Expected: schema: "../old/schema.json"
Action taken: PAUSED for investigation
```

### 3. Verify at Checkpoints

After completing each phase, validate checkpoint:

| Check | Method | Result |
|-------|--------|--------|
| Structural | Read modified artifacts | Pass/Fail |
| References | Trace links to targets | Pass/Fail |
| Consistency | Compare related content | Pass/Fail |

Document checkpoint result:

```markdown
## Checkpoint CP1 (after Phase 1)
- Corrections completed: CG1-C1, CG1-C2, CG3-C1
- Structural validation: PASS
- Reference validation: PASS
- Status: PROCEED to Phase 2
- Timestamp: [when verified]
```

### 4. Handle Failures

When correction or checkpoint fails:

#### Minor Failures (recoverable inline)

1. Identify discrepancy between expected and observed state
2. Adjust correction to handle actual state
3. Apply adjusted correction
4. Re-verify and proceed

Document adjustment:
```markdown
Adjustment: Target line differed from plan
Resolution: Used actual content as baseline, applied equivalent change
```

#### Major Failures (require decision)

1. Pause execution
2. Document failure details
3. Assess options:
   - **Fix forward**: Correction approach needs revision
   - **Rollback**: Return to previous checkpoint
   - **Investigate**: Root cause unclear

```markdown
## Execution Paused at CG2-C1

### Failure Details
- Expected: Path "../old/schema.json"
- Observed: Path "../../legacy/schema.json"
- Implication: Source artifact changed since analysis

### Options
1. Update correction to use observed baseline → proceed
2. Re-run issue analysis → restart planning
3. Rollback to CP0 → investigate artifact changes
```

### 5. Execute Rollback When Needed

Follow rollback strategy from plan:

1. **Identify target checkpoint**: Most recent verified stable state
2. **Apply rollback actions**: Revert changes in reverse order
3. **Verify rollback**: Confirm target checkpoint state restored
4. **Document rollback**: Cause, actions taken, resulting state

```markdown
## Rollback Executed

From: CP2 (failed)
To: CP1 (verified stable)
Cause: Reference validation failed - target file not created
Actions: Reverted CG2-C1, CG2-C2
Verification: CP1 state restored, structural checks pass
Next: Investigate CG3-C1 (schema creation) before retry
```

### 6. Complete Execution

After all phases complete and final checkpoint passes:

1. **Verify against original findings**: Check each finding from analysis
2. **Run validation subset**: Re-validate corrected areas
3. **Document remaining issues**: Any findings not resolved
4. **Produce completion report**

```markdown
## Correction Execution Complete

### Summary
- Corrections planned: [N]
- Corrections successful: [N]
- Corrections failed/skipped: [N]
- Rollbacks executed: [N]

### Findings Resolution
| Finding | Status | Correction |
|---------|--------|------------|
| F1 | Resolved | CG1-C1 |
| F2 | Resolved | CG2-C1 |
| F3 | Unresolved | Requires manual review |

### Remaining Issues
- [Issue]: [Reason unresolved]

### Verification Results
- Re-validation: [Pass/Partial/Fail]
- New findings: [None/List]

### Artifacts Modified
- [list of files changed with change summary]
```

## Decision Points

**Artifact state differs from plan**: Assess whether difference is cosmetic or substantive. Cosmetic differences allow adjustment; substantive changes may require re-analysis.

**Checkpoint partially passes**: Some validations pass, others fail. Assess whether failures block dependent corrections. May proceed with documented exceptions.

**Correction has unintended effects**: Verification reveals changes beyond planned scope. Investigate cause before proceeding.

**Multiple consecutive failures**: Pattern suggests plan assumptions invalid. Consider full rollback and re-analysis over incremental fixes.

## Success Criteria

- Baseline verified before modifications
- Each correction logged with result
- Checkpoints verified between phases
- Failures handled with documented decisions
- Rollbacks restore verified prior state
- Completion report documents all outcomes
- Original findings resolved or explicitly documented as remaining

## Common Issues

**Rushing past verification**: Pressure to complete leads to skipped checks. Early error detection prevents compounding failures.

**Incomplete rollback**: Partial reversion leaves inconsistent state. Verify rollback reaches checkpoint condition, not approximate state.

**Lost context during pause**: Execution suspended without sufficient documentation. Record state fully when pausing for decisions.

**Silent failures**: Correction appears to succeed but introduces new issue. Verification must cover correctness, not just completion.

**Drift from plan**: Accumulating adjustments diverge from original plan. Significant drift suggests re-planning rather than continued adjustment.
