# Execution Checkpoints Procedure

Verify system integrity at defined checkpoints during implementation. This procedure catches errors early, confirms progress, and maintains rollback capability throughout modification.

## Prerequisites

- Change plan completed with defined checkpoints
- Validation methods identified for each checkpoint
- Rollback strategy documented
- System in known baseline state (CP0)

## Steps

### 1. Establish Baseline Validation

Before any modification, verify baseline state:

- **Artifact state**: Target artifacts exist and match expected content
- **System state**: System operational if applicable (services running, builds passing)
- **Recovery state**: Backup or version control in clean state

Document baseline validation result:

```markdown
## Baseline (CP0)
- Artifacts: [list of files verified]
- System: [operational status]
- Recovery: [version control status, backup location]
- Timestamp: [when verified]
```

### 2. Execute Changes to Next Checkpoint

Apply changes in planned sequence until reaching next checkpoint:

1. **Announce change**: Log which change is starting
2. **Apply modification**: Execute the planned change
3. **Verify immediate effect**: Confirm change applied (file written, artifact created)
4. **Continue or pause**: Move to next change or pause at checkpoint

**During execution**, track:
- Changes completed successfully
- Changes attempted but failed
- Changes not yet attempted

### 3. Validate Checkpoint

At each checkpoint, perform validation:

#### Structural Validation

Verify artifacts exist and have expected structure:

| Check | Method | Pass Criteria |
|-------|--------|---------------|
| Files exist | List/read target paths | All expected artifacts present |
| Content valid | Parse/validate artifacts | No syntax or format errors |
| References resolve | Trace dependencies | No broken references |

#### Behavioral Validation

Verify system behavior when applicable:

| Check | Method | Pass Criteria |
|-------|--------|---------------|
| Build succeeds | Execute build process | No errors |
| Tests pass | Run test suite | Expected tests pass |
| System operational | Health check, smoke test | Core functions work |

#### Consistency Validation

Verify internal consistency:

| Check | Method | Pass Criteria |
|-------|--------|---------------|
| Data integrity | Cross-reference validation | No orphaned references |
| Configuration valid | Schema validation | Config matches schema |
| State coherent | State inspection | No contradictory state |

### 4. Record Checkpoint Result

Document validation outcome:

```markdown
## Checkpoint [N]
- Changes completed: [C1, C2, ...]
- Structural: [Pass/Fail with details]
- Behavioral: [Pass/Fail with details]
- Consistency: [Pass/Fail with details]
- Status: [Proceed / Rollback / Investigate]
- Timestamp: [when validated]
```

### 5. Decide Next Action

Based on checkpoint result:

| Validation Result | Action |
|-------------------|--------|
| All pass | Proceed to next checkpoint |
| Minor issues, understood | Fix inline, revalidate, proceed |
| Major issues, recoverable | Rollback to previous checkpoint |
| Major issues, unclear cause | Investigate before rollback |

### 6. Handle Failures

When validation fails:

1. **Document failure**: Specific check that failed, observed vs expected
2. **Assess scope**: Which changes contributed to failure
3. **Choose recovery**:
   - **Fix forward**: Issue is minor, fix and revalidate
   - **Rollback**: Restore to previous checkpoint
   - **Investigate**: Failure cause unclear, gather information

**Rollback execution**:
1. Apply rollback actions from plan
2. Revalidate at target checkpoint
3. Document rollback and cause

## Decision Points

**Partial validation failure**: Some checks pass, others fail. Assess whether failures block progress or can be addressed independently.

**Validation method unavailable**: Planned validation cannot run (test suite broken, build system unavailable). Document skip with rationale, proceed with caution.

**Unexpected side effects**: Validation reveals changes beyond planned scope. Investigate cause before proceeding.

**Performance degradation**: Functional validation passes but system slower. Document as known issue if acceptable, or treat as failure if outside tolerance.

**External dependency failure**: Validation fails due to external system. Distinguish external from internal issues before rollback decision.

## Success Criteria

- Baseline validated before modifications begin
- Each checkpoint validated with documented results
- Failures trigger appropriate response (fix, rollback, investigate)
- Rollback restores to validated prior state
- All checkpoint validations recorded with timestamps

## Common Issues

**Skipping validation under pressure**: Time constraints tempt skipping checks. Catching issues late costs more than validation time.

**Validation too coarse**: Single "it works" check misses specific failures. Break validation into structural, behavioral, consistency components.

**Accumulated drift**: Small issues deferred at each checkpoint compound. Address issues when detected, not after all changes complete.

**Rollback doesn't fully restore**: Partial cleanup leaves artifacts or state. Verify rollback reaches validated prior state, not just "closer to prior."

**Validation passes but wrong behavior**: Checks don't cover actual requirements. Review validation coverage when unexpected issues emerge post-completion.

**Flaky validation**: Intermittent failures mask real issues or block progress. Distinguish deterministic failures from environmental noise.
