# Validation Before Execution

Validate plans, inputs, and approaches before committing to execution. Errors discovered late are expensive to fix. A validation failure before execution means revising a plan; a failure after execution means redoing work.

## Validation Points

Validation occurs at multiple stages, each catching different error categories.

### Input Validation

Before execution, verify:

- Required context files exist and are accessible
- Input format matches expectations
- Prerequisites are satisfied
- Dependencies are available

Rejecting bad input costs almost nothing. Processing bad input wastes the entire execution.

### Plan Validation

Before orchestration, verify:

- Phases are correctly ordered
- Dependencies between agents are satisfied
- Parallel groups contain only independent agents
- Expected outputs are defined for each phase

A flawed plan produces flawed results regardless of execution quality.

### Approach Validation

Before implementation, verify:

- Approach aligns with research findings
- Patterns match existing codebase conventions
- No conflicts with established principles
- Edge cases are considered

The wrong approach executed well still produces wrong results.

## Validation Workflow

Validation forms checkpoints between stages:

```
Input → Validate → Plan → Validate → Execute → Validate → Output
          ↓                 ↓                    ↓
        [Fix]            [Revise]            [Correct]
```

Failures loop back to correction before proceeding. The workflow does not advance until validation passes.

## Multi-Round Validation

Complex artifacts benefit from multiple passes, each checking different aspects that are relevant for the particular artifact and task or issue. Separate rounds prevent cognitive overload. Checking everything simultaneously leads to missed issues.

## Validation Reports

Document validation outcomes in structured reports:

- What was validated
- Issues found
- Corrections applied
- Final validation outcome (pass/fail)

Reports provide an audit trail and inform future validation efforts. Patterns in validation failures reveal systematic problems.

## Correction Workflow

When validation fails:

1. Document specific problems found
2. Determine required corrections
3. Apply changes
4. Re-validate to confirm resolution

Do not proceed until validation passes. Skipping re-validation after corrections defeats the purpose.

## Error Cost by Stage

| Stage | Cost |
|-------|------|
| Input validation | Minimal—reject and request correct input |
| Plan validation | Low—revise plan before execution |
| Post-execution validation | High—redo completed work |
| Production discovery | Very high—user impact, reputation damage |

## Common Mistakes

| Mistake | Consequence |
|---------|-------------|
| Execute first, validate later | Expensive corrections after wasted effort |
| Skip validation for "simple" tasks | Hidden errors propagate through the system |
| Single validation round | Misses issues caught by different validation types |
| No validation reports | Lost audit trail, repeated mistakes |
| Proceed despite validation failure | Compounding errors through subsequent stages |

## Related Principles

- **Research-first**: Research validates the approach before implementation begins
- **Context efficiency**: Validation prevents wasted tokens on doomed executions
