# Checkpoint Definition Procedure

Establish verification points within an execution plan where progress can be confirmed and rollback remains practical. This procedure determines checkpoint placement, defines verification criteria for each checkpoint, and specifies rollback triggers that govern when to revert rather than proceed.

## Prerequisites

- Execution sequence defined with ordered steps or phases
- Success criteria established for the overall plan
- Dependency map available showing relationships between planned changes
- Rollback mechanisms identified (version control, backups, undo procedures)
- Risk assessment completed or risk tolerance understood

## Steps

### 1. Identify Checkpoint Candidates

Examine the execution sequence for natural verification boundaries. Candidates occur where the system reaches a stable intermediate state -- a point where completed work functions independently of remaining work.

| Candidate Type | Indicator | Example |
|----------------|-----------|---------|
| Dependency boundary | Downstream steps depend on prior output | Schema created before consumers reference it |
| Phase transition | Execution shifts from one category of work to another | Configuration complete, modification begins |
| Risk threshold | Subsequent steps carry higher failure probability | Stable state before high-risk modification |
| Integration point | Independent work streams converge | Parallel changes merge into shared artifact |
| External handoff | Execution pauses for external input or review | Artifacts delivered for stakeholder approval |

List candidates with their position in the execution sequence and the rationale for each.

### 2. Determine Checkpoint Density

Balance verification thoroughness against execution overhead. Too many checkpoints slow execution; too few leave long unverified stretches where errors accumulate silently.

| Factor | Fewer Checkpoints | More Checkpoints |
|--------|-------------------|------------------|
| Risk level | Low-risk, well-understood changes | High-risk or uncertain changes |
| Rollback cost | Inexpensive to reverse any step | Expensive or destructive to reverse |
| Change interdependence | Independent, isolated changes | Tightly coupled changes |
| Execution duration | Short execution (minutes) | Long execution (hours or days) |
| Artifact criticality | Low-impact artifacts | High-impact or shared artifacts |

Apply the following density guidelines:

- **Minimum**: Start checkpoint (baseline) and end checkpoint (completion). Every plan has at least two.
- **Standard**: One checkpoint per dependency boundary or phase transition. Typical plans have 3-5 checkpoints.
- **High-density**: Checkpoint after each significant change. Use for first-time execution patterns, high-risk domains, or unfamiliar artifact types.

Select candidates from Step 1 that satisfy the density appropriate to the plan's risk profile. Discard candidates that add verification overhead without meaningfully reducing rollback distance.

### 3. Define Verification Criteria

For each selected checkpoint, specify what must be true before execution proceeds. Verification criteria answer the question: "How do we confirm the system is in the expected state?"

Structure criteria in three categories:

**Structural criteria** confirm artifacts exist in expected form:
- Target artifacts present at expected locations
- Artifact content matches expected structure or format
- References between artifacts resolve correctly

**Behavioral criteria** confirm the system operates as expected:
- Processing completes without errors
- Output matches expected results for known inputs
- Dependent components interact correctly

**Consistency criteria** confirm internal coherence:
- No orphaned references from removed or renamed artifacts
- Configuration values align with artifact expectations
- State across related artifacts remains coherent

Not every checkpoint requires all three categories. Match verification depth to checkpoint importance:

| Checkpoint Position | Minimum Verification |
|---------------------|---------------------|
| Baseline (first) | All three categories -- establishes known-good state |
| Mid-execution | Structural plus one other category relevant to recent changes |
| Completion (final) | All three categories -- confirms full plan success |

Document criteria in verifiable form. Each criterion should produce a binary pass/fail result, not a subjective assessment.

### 4. Establish Rollback Triggers

Define conditions under which execution should revert to the previous checkpoint rather than attempt to fix the issue and continue. Rollback triggers prevent the common failure mode of accumulating partial fixes that obscure the original problem.

Specify triggers at two levels:

**Automatic triggers** -- always roll back:
- Verification criteria fail for artifacts not modified in the current segment
- Changes produce side effects outside the planned modification scope
- Rollback mechanism itself becomes unavailable or compromised

**Judgment triggers** -- assess before deciding:
- One or more verification criteria fail for recently modified artifacts
- Execution reveals information that invalidates plan assumptions
- Elapsed time or resource consumption exceeds planned allocation

For each checkpoint, document:

```markdown
## Checkpoint {N}: {Description}

**Preceding changes**: {list of changes since previous checkpoint}

**Verification criteria**:
- Structural: {criteria}
- Behavioral: {criteria}
- Consistency: {criteria}

**Rollback trigger**: {condition that warrants reverting to checkpoint N-1}
**Rollback action**: {specific steps to restore previous checkpoint state}
**Proceed condition**: {all criteria that must pass before continuing}
```

### 5. Validate Checkpoint Coverage

Review the complete set of checkpoints against the execution plan to confirm adequate coverage. Verify that:

- Every high-risk step has a checkpoint immediately preceding it
- No segment between checkpoints exceeds the acceptable rollback distance for the plan's risk level
- Rollback from any checkpoint produces a functional intermediate state, not a broken one
- The final checkpoint's verification criteria match or exceed the plan's overall success criteria
- Checkpoint verification methods are available and practical at each point in the sequence

Gaps in coverage indicate either missing checkpoints or verification criteria that require revision. Add checkpoints where coverage is insufficient; consolidate where adjacent checkpoints provide redundant verification.

### 6. Document Checkpoint Sequence

Produce the checkpoint definition as a section of the execution plan:

```markdown
## Checkpoints

| ID | After | Description | Verification | Rollback To |
|----|-------|-------------|--------------|-------------|
| CP0 | -- | Baseline state | Full validation | N/A |
| CP1 | {changes} | {state description} | {criteria summary} | CP0 |
| CP2 | {changes} | {state description} | {criteria summary} | CP1 |
| CPn | {changes} | Completion | Full validation | CP{n-1} |
```

Include detailed verification criteria and rollback actions in subsections below the summary table when criteria exceed single-line descriptions.

## Decision Points

**Single-step plan**: Use baseline and completion checkpoints only. Intermediate checkpoints add overhead without value when the entire plan is one atomic change.

**Verification method unavailable at planned checkpoint**: Move the checkpoint to the nearest point where verification is practical. If no practical verification exists for a long segment, break the segment into smaller steps that enable verification.

**Checkpoint fails but rollback is expensive**: Assess whether fixing forward costs less than rollback. Document the decision and rationale. If fixing forward, add a new checkpoint immediately after the fix to verify recovery before continuing.

**Execution reveals unplanned dependencies**: Add checkpoints at newly discovered dependency boundaries. Revise subsequent checkpoint verification criteria to account for the new dependency.

**Stakeholder requires approval gate**: Treat approval as an external handoff checkpoint. Define verification criteria that include the approval artifact (sign-off, review completion) alongside technical criteria.

## Success Criteria

- Every execution plan has at minimum a baseline and completion checkpoint
- Checkpoint density matches the plan's risk profile
- Each checkpoint has verifiable pass/fail criteria, not subjective assessments
- Rollback triggers defined for each checkpoint with specific restoration steps
- No segment between checkpoints exceeds acceptable rollback distance
- Final checkpoint verification criteria satisfy the plan's overall success criteria

## Common Issues

**Checkpoints without actionable criteria**: Verification stated as "confirm everything works" provides no guidance. Break vague criteria into specific structural, behavioral, and consistency checks with binary outcomes.

**Rollback actions that assume clean state**: Partial execution may leave artifacts, temporary state, or side effects that rollback procedures do not address. Validate rollback actions against realistic mid-execution scenarios, not idealized assumptions.

**Checkpoint density mismatch**: Over-checkpointing low-risk plans wastes execution time on unnecessary verification. Under-checkpointing high-risk plans allows errors to propagate across many changes before detection. Revisit density when risk assessment changes during execution.

**Verification criteria that test the wrong thing**: Criteria that confirm the change was applied without confirming the change produced the intended effect. Behavioral criteria should verify outcomes rather than mere presence of modifications.

**Skipping baseline validation**: Proceeding without confirming the starting state is known-good. Subsequent checkpoints compare against baseline -- an unverified baseline makes all comparisons unreliable.
