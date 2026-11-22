# Completion Reporting Procedure

Document implementation outcomes after all changes complete. This procedure creates an authoritative record of what changed, how it deviated from plan, and what concerns remain for future reference.

## Prerequisites

- All planned changes executed or explicitly abandoned
- Final checkpoint validated
- Implementation notes captured during execution
- Access to original plan for comparison

## Steps

### 1. Summarize Outcome

State the implementation result in 2-3 sentences:

- What was accomplished
- Whether plan was followed or modified
- Overall success status

Example: "Added validation logic to processor module. Plan followed with one deviation: schema location changed from `schemas/` to `config/schemas/` due to existing convention. All checkpoints passed."

### 2. Document Changes Made

Create authoritative record of modifications:

```markdown
## Changes

| Target | Type | Description | Status |
|--------|------|-------------|--------|
| config/schemas/validation.json | Create | Validation schema definition | Complete |
| src/processor.py | Modify | Added validation function | Complete |
| src/processor.py:45-67 | Modify | Integrated validation call | Complete |
| tests/test_validation.py | Create | Validation test cases | Complete |
```

**Include**:
- Exact paths modified
- Line ranges for targeted modifications
- Artifact types created or deleted
- Final status (Complete, Partial, Abandoned)

### 3. Record Deviations

Document differences between plan and execution:

```markdown
## Deviations

### D1: Schema Location Changed
- **Planned**: `schemas/validation.json`
- **Actual**: `config/schemas/validation.json`
- **Reason**: Existing project convention places schemas under `config/`
- **Impact**: None—path references updated accordingly

### D2: Additional Test Case Added
- **Planned**: 3 test cases
- **Actual**: 4 test cases
- **Reason**: Edge case discovered during implementation
- **Impact**: Improved coverage
```

**Deviation types**:
- **Scope change**: More or fewer changes than planned
- **Sequence change**: Different execution order
- **Target change**: Different artifact than planned
- **Approach change**: Different technique to achieve same outcome

### 4. Document Remaining Concerns

Identify issues not fully resolved:

```markdown
## Remaining Concerns

### Known Limitations
- Validation does not cover nested objects (out of scope)
- Performance impact not measured under load

### Deferred Work
- Add validation error localization
- Extend schema for additional field types

### Risks
- External API may return unexpected formats not covered by schema
- Schema version management not addressed
```

**Concern categories**:
- **Known limitations**: Intentional scope boundaries
- **Deferred work**: Identified improvements not implemented
- **Risks**: Potential future issues
- **Technical debt**: Shortcuts taken for expediency

### 5. Capture Learnings

Record insights for future implementations:

```markdown
## Learnings

### What Worked
- Checkpoint validation caught config path issue early
- Dependency ordering prevented broken intermediate states

### What Didn't Work
- Initial schema location assumption required mid-execution change
- Test framework setup took longer than expected

### Recommendations
- Check existing conventions before planning artifact locations
- Include framework setup in planning estimates
```

### 6. Compile Final Report

Assemble complete report:

```markdown
# Implementation Report

## Summary
[2-3 sentences from Step 1]

## Changes
[Table from Step 2]

## Deviations
[List from Step 3]

## Validation Results
[Final checkpoint status, key validation outcomes]

## Remaining Concerns
[List from Step 4]

## Learnings
[List from Step 5]

## References
- Plan: [link to original plan]
- Checkpoint logs: [link or inline]
- Related documentation: [updated docs]
```

## Decision Points

**Partial completion**: Some changes complete, others abandoned. Document what completed, why remaining changes were abandoned, and whether partial state is acceptable.

**Unplanned changes required**: Execution discovered necessary changes not in plan. Document as deviations with rationale.

**Concerns vs blockers**: Distinguish issues that allow completion from issues that should have blocked. Blockers discovered post-completion indicate planning gaps.

**Report audience**: Adjust detail level for audience—technical peers need different information than stakeholders. Include technical details in appendix if needed.

## Success Criteria

- Outcome summary captures essential result
- All changes documented with locations and status
- Deviations explained with rationale and impact
- Remaining concerns categorized and actionable
- Learnings recorded for future reference
- Report complete and self-contained

## Common Issues

**Omitting abandoned work**: Unfinished changes still matter—they indicate scope change or discovered blockers. Document why work stopped.

**Vague deviation rationale**: "Changed because it was better" doesn't help future planners. State specific reason: convention, discovered requirement, technical constraint.

**Ignoring negative learnings**: Failures and difficulties provide more learning than successes. Document what didn't work and why.

**Report too long**: Excessive detail obscures key information. Use summary for essentials, appendix for details.

**Missing validation evidence**: Stating "tests pass" without specifics. Include which tests, pass rates, and any exceptions.

**Deferred concerns lost**: Remaining concerns documented but never acted on. Ensure concerns route to appropriate tracking system.
