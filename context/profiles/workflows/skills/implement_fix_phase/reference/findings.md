# Findings Implementation Workflow

Variant of implementation workflow for applying corrections identified by validation reports. Follows the standard implementation phases with discovery adapted to parse validation findings.

## Workflow Overview

```
Phase 1: Findings Resolution
         ↓
Phase 2: Correction Planning
         ↓
Phase 3: Correction Implementation ←──┐
         ↓                            │
Phase 4: Re-Validation                │
         ↓                            │
    Issues Found? ─────Yes────────────┘
         ↓ No
      Complete
```

## When to Use

Use findings implementation workflow when:

- Validation reports identify issues requiring correction
- Scope is defined by validation findings rather than task requirements
- Corrections must be applied systematically with verification

## Phase Structure

### Phase 1: Findings Resolution

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_findings_resolution.md" />

**Workflow-specific notes**:
- Loads validation report specified by `--input` or most recent validation report
- Filters findings by severity threshold (`--severity` parameter)
- Groups findings by affected artifact for systematic planning

### Phase 2: Correction Planning

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/correction.md`

**Workflow-specific notes**:
- Plans minimal corrections addressing each finding
- Sequences corrections to avoid conflicts
- Does not expand scope beyond identified findings

### Phase 3: Correction Implementation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_edit.md" />

**Workflow-specific notes**:
- Applies corrections according to plan
- Uses Opus model for modification quality
- Documents each correction with finding reference
- When a correction requires root-cause work beyond the mechanical fix, apply the diagnosis discipline first — `ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/diagnosing_bugs.md`

### Phase 4: Re-Validation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_general.md" />
`ctx:github.com/ctxmod/agentic/context::shared::remediation_phase@workflows/reference/targeted.md`

**Workflow-specific notes**:
- Validates using same criteria as original validation
- Checks for regression or new issues introduced
- Validation loop (max 3 iterations) until findings resolved

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Phase 1: Findings Resolution
- Receives: {inherited}, validation report path
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-findings-resolution.md

Phase 2: Correction Planning
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-correction-plan.md

Phase 3: Correction Implementation
- Receives: {inherited}, Phase 2 output
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-correction-implementation.md

Phase 4: Re-Validation
- Receives: {inherited}, modified artifacts, original validation criteria
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-revalidation.md
```

## Workflow Variants

### Strict Findings Implementation

- Terminates only on zero findings
- No severity threshold adjustment
- Maximum iterations: 3
- Use case: Compliance requirements

### Pragmatic Findings Implementation

- Filters by severity threshold
- Accepts warnings below threshold
- Maximum iterations: 3
- Use case: General quality improvement

## Error Handling

### Unresolved Findings (Exceeded Iterations)

If validation loop exceeds maximum iterations:

1. Report persistent findings with remediation history
2. Document what remains unresolved
3. Assess partial success value
4. Propose manual intervention or scope reduction
5. Await user decision

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Expanding scope beyond findings | Scope creep, iteration exhaustion | Fix only identified findings |
| Applying corrections without planning | Conflicts, incomplete fixes | Plan corrections first |
| Skipping re-validation | Unconfirmed outcomes | Always verify after correction |
| Fixing warnings before errors | Errors block acceptance | Prioritize by severity |
