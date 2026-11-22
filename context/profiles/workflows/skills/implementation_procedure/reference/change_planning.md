# Change Planning Procedure

Establish the order of modifications, identify dependencies between changes, and define rollback strategy. This procedure creates an execution plan that minimizes intermediate broken states and enables recovery at any point.

## Prerequisites

- Context assessment completed (sufficiency confirmed)
- Modification scope defined (what will change)
- Target artifacts accessible and in known state
- Recovery mechanism verified (backup, version control, or restore process)

## Steps

### 1. Inventory Required Changes

List all modifications with granularity appropriate for tracking:

```markdown
| ID | Target | Change Type | Description |
|----|--------|-------------|-------------|
| C1 | config.yaml | Modify | Update threshold value |
| C2 | processor.py | Modify | Add validation logic |
| C3 | schema.json | Create | Define new validation schema |
| C4 | tests/ | Create | Add validation test cases |
```

**Change types**: Create, Modify, Delete, Move, Rename

### 2. Map Change Dependencies

Identify which changes depend on others:

- **Data dependencies**: C2 requires schema from C3
- **Structural dependencies**: Test file C4 requires function in C2
- **Configuration dependencies**: C2 behavior depends on C1 values

Build dependency graph:

```
C3 (schema) → C2 (uses schema) → C4 (tests C2)
C1 (config) → C2 (reads config)
```

### 3. Determine Execution Order

Derive sequence from dependency graph using topological sort:

1. **Independent changes first**: No incoming dependencies
2. **Dependent changes follow**: After their dependencies complete
3. **Parallel-safe changes**: Independent changes can execute simultaneously

Example sequence from above graph:
1. C1, C3 (parallel-safe, no dependencies)
2. C2 (depends on C1, C3)
3. C4 (depends on C2)

### 4. Identify Checkpoints

Define stable states where system remains functional:

| Checkpoint | After Changes | System State |
|------------|---------------|--------------|
| CP0 | None | Baseline (current) |
| CP1 | C1, C3 | Config updated, schema added, unused |
| CP2 | C2 | Processor uses new config and schema |
| CP3 | C4 | Tests added and passing |

**Checkpoint criteria**:
- System functional (no broken intermediate state)
- Rollback straightforward (clear what to undo)
- Progress preserved (meaningful work completed)

### 5. Define Rollback Strategy

For each checkpoint, document restoration approach:

| From | To | Rollback Action |
|------|-----|-----------------|
| CP1 | CP0 | Revert C1, delete C3 |
| CP2 | CP1 | Revert C2 |
| CP3 | CP2 | Delete C4 |

**Rollback types**:
- **Revert**: Restore previous content
- **Delete**: Remove created artifact
- **Recreate**: Restore deleted artifact from backup

### 6. Document the Plan

Produce execution plan:

```markdown
## Change Plan

### Changes
[Table from Step 1]

### Dependencies
[Graph or list from Step 2]

### Execution Sequence
1. [Change ID]: [Description] — [Dependencies]
2. ...

### Checkpoints
[Table from Step 4]

### Rollback Strategy
[Table from Step 5]

### Assumptions
[Conditions assumed true during execution]
```

## Decision Points

**Circular dependencies detected**: Refactor changes to break cycle. Extract shared component, or combine changes into atomic unit.

**Long dependency chain**: Identify intermediate checkpoints. Chains longer than 5 changes without checkpoint increase rollback complexity.

**Uncertain dependency**: Treat as dependency until verified otherwise. False dependencies add ordering constraints but remain safe. Missed dependencies cause failures.

**Parallel execution available**: Group independent changes for simultaneous execution when isolation is verified (no shared state, separate artifacts).

**External system dependencies**: Changes depending on external systems require coordination. Document external preconditions and verification steps.

## Success Criteria

- All changes inventoried with type and target
- Dependencies mapped without cycles
- Execution sequence derived from dependencies
- Checkpoints defined at stable states
- Rollback strategy documented for each checkpoint
- Plan reviewed for completeness before execution

## Common Issues

**Implicit dependencies**: Changes appear independent but share runtime state, configuration, or external systems. Trace beyond direct references.

**Missing intermediate state**: Jumping from checkpoint to checkpoint without verifying intermediate states work. Test after each non-trivial change.

**Rollback assumes clean state**: Recovery may fail if intermediate artifacts remain. Document cleanup steps for partial rollback.

**Underestimating create/delete complexity**: New artifacts need directory structure; deleted artifacts may have consumers. Verify path existence and orphaned references.

**Plan drift during execution**: Discovered issues change the plan. Document deviations and reassess remaining sequence when plan changes.
