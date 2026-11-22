# Refactor Implementation Workflow

Variant of implementation workflow for reorganizing artifacts while preserving behavior. Emphasizes baseline discovery and equivalence verification.

## Workflow Overview

```
Phase 1: Baseline Discovery (parallel)
         ├── Structure discovery
         └── Consumer discovery
         ↓
Phase 2: Refactor Planning
         ↓
Phase 3: Transformation ←───────┐
         ↓                      │
Phase 4: Equivalence Verification│
         ├── Structure           │
         ├── References          │
         └── Behavior            │
         ↓                      │
    Issues Found? ─────Yes───────┘
         ↓ No
      Complete
```

## When to Use

Use refactor implementation workflow when:

- Reorganizing artifacts without changing behavior
- Renaming, moving, extracting, or consolidating components
- Behavior preservation is critical
- Consumer updates must occur atomically with target changes

## Phase Structure

### Phase 1: Baseline Discovery

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_baseline.md" />

**Workflow-specific notes**:
- Parallel discovery: structure baseline + consumer baseline
- Captures comprehensive baseline for comparison
- Documents all consumers requiring updates

### Phase 2: Refactor Planning

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/general.md`

**Workflow-specific notes**:
- Plans transformation steps in safe sequence
- Ensures behavior preservation strategy
- Plans consumer updates alongside target changes
- Defines rollback strategy

### Phase 3: Transformation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_edit.md" />

**Workflow-specific notes**:
- Applies planned transformation only
- Updates all consumers atomically with targets
- Preserves behavior at each step
- Uses Opus model for transformation quality

### Phase 4: Equivalence Verification

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_equivalence.md" />

**Workflow-specific notes**:
- Parallel verification: structure + references + behavior (if applicable)
- Compares transformed state to baseline
- Checks for unplanned changes
- Runs behavioral verification when available

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Phase 1: Baseline Discovery (parallel)
- Receives: {inherited}, scope
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-baseline-structure.md
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-baseline-consumers.md

Phase 2: Refactor Planning
- Receives: {inherited}, Phase 1 outputs
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-refactor-plan.md

Phase 3: Transformation
- Receives: {inherited}, Phase 2 output
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-transformation.md

Phase 4: Equivalence Verification (parallel)
- Receives: {inherited}, transformed artifacts, Phase 1 baselines
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-verification-structure.md
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-verification-references.md
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-verification-behavior.md (conditional)
```

## Workflow Variants

### Strict Refactor

- Requires behavioral verification
- Zero tolerance for broken references
- Maximum iterations: 5
- Use case: Critical systems

### Pragmatic Refactor

- Behavioral verification when available
- Structural and reference verification mandatory
- Maximum iterations: 3
- Use case: General refactoring

### Incremental Refactor

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

- Large-scope refactoring split into increments
- Verification between increments
- Each increment follows full workflow

## Error Handling

### Transformation Failure

If transformation fails:

1. Report transformation failure with context
2. Document partial changes if any
3. Assess rollback necessity using baseline
4. Propose recovery or alternative approach
5. Await user decision

### Verification Failure (Exceeded Iterations)

If equivalence verification fails after maximum iterations:

1. Report persistent issues with remediation history
2. Compare baseline to current state
3. Assess behavior preservation status
4. Propose manual intervention or rollback
5. Await user decision

## Quality Criteria

| Criterion | Validation Method |
|-----------|-------------------|
| Structure matches plan | Phase 4a compares to plan |
| References intact | Phase 4b verifies all resolve |
| Behavior preserved | Phase 4c runs available verification |
| No unplanned changes | Phase 4a checks scope boundaries |

## Risk Mitigation

### Pre-Transformation Safeguards

- **Baseline documentation**: Comprehensive baseline enables comparison and rollback
- **Scope boundaries**: Explicit include/exclude lists
- **Consumer catalog**: Complete consumer inventory

### During Transformation

- **Atomic updates**: Consumers updated with targets
- **Logging**: Record each transformation step
- **Checkpoints**: Verify after logical units

### Post-Transformation

- **Baseline comparison**: Verify only planned changes occurred
- **Reference verification**: Confirm no broken references
- **Behavior check**: Run verification suites

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping baseline discovery | No comparison reference | Establish baseline first |
| Transforming without planning | Uncontrolled changes | Plan transformation steps |
| Updating targets without consumers | Broken references | Update atomically |
| Skipping equivalence verification | Unconfirmed preservation | Verify equivalence |
| Expanding scope during transformation | Scope creep | Transform planned scope only |
