# Approach Selection Procedure

Evaluate candidate approaches from analysis recommendations and select the optimal execution strategy. This procedure scores alternatives against weighted criteria, assesses trade-offs between competing options, and validates the selected approach against known constraints before committing to an execution plan.

## Prerequisites

- Analysis report available with two or more recommended approaches
- Evaluation criteria identified or derivable from project goals
- Constraint inventory accessible (resource limits, time boundaries, capability gaps)
- Stakeholder priorities documented or inferable from context

## Steps

### 1. Extract Candidate Approaches

Collect all viable approaches from analysis output. Each candidate needs a clear description, the analysis evidence supporting it, and the conditions under which it performs best.

```markdown
| ID | Approach | Source | Best When |
|----|----------|--------|-----------|
| A1 | [Description] | [Analysis finding reference] | [Optimal conditions] |
| A2 | [Description] | [Analysis finding reference] | [Optimal conditions] |
| A3 | [Description] | [Analysis finding reference] | [Optimal conditions] |
```

Discard approaches that analysis flagged as infeasible. Retain borderline candidates for evaluation -- premature elimination reduces decision quality.

### 2. Define Evaluation Criteria

Establish the dimensions along which candidates will be scored. Criteria derive from project goals, constraints, and stakeholder priorities.

Common criteria categories:

| Category | Example Criteria |
|----------|-----------------|
| Effectiveness | Likelihood of achieving desired outcome |
| Effort | Resources, time, and coordination required |
| Risk | Probability and severity of failure modes |
| Reversibility | Ease of course correction if approach underperforms |
| Compatibility | Fit with existing patterns, conventions, and dependencies |

Select 3-5 criteria. Fewer than three provides insufficient differentiation. More than five dilutes scoring precision without improving decision quality.

### 3. Assign Criteria Weights

Distribute weight across criteria to reflect relative importance. Weights must sum to 1.0.

```markdown
| Criterion | Weight | Rationale |
|-----------|--------|-----------|
| Effectiveness | 0.35 | Primary goal alignment |
| Risk | 0.25 | Failure cost is high |
| Effort | 0.20 | Resource constraints apply |
| Reversibility | 0.10 | Rollback paths exist |
| Compatibility | 0.10 | Existing patterns are flexible |
```

When stakeholder priorities conflict, assign higher weight to the dimension where error costs are greatest. Document the rationale for weight distribution so subsequent reviewers can assess whether weights reflect current conditions.

### 4. Score Candidates

Rate each candidate on each criterion using a consistent scale (1-5):

| Score | Meaning |
|-------|---------|
| 5 | Excellent fit for this criterion |
| 4 | Good fit with minor limitations |
| 3 | Adequate fit with notable trade-offs |
| 2 | Poor fit requiring workarounds |
| 1 | Incompatible or unacceptable |

Produce a scoring matrix:

```markdown
| Criterion (Weight) | A1 | A2 | A3 |
|---------------------|----|----|-----|
| Effectiveness (0.35) | 4 | 3 | 5 |
| Risk (0.25) | 3 | 4 | 2 |
| Effort (0.20) | 4 | 5 | 2 |
| Reversibility (0.10) | 3 | 4 | 3 |
| Compatibility (0.10) | 5 | 3 | 4 |
| **Weighted Total** | **3.75** | **3.75** | **3.35** |
```

Calculate weighted totals: sum of (score x weight) for each candidate.

### 5. Assess Trade-offs Between Leaders

When two or more candidates score within 0.5 points of each other, raw scores alone do not determine the selection. Examine qualitative trade-offs between the leading candidates.

For each pair of closely scored candidates, document:

- **Where A1 outperforms A2**: Specific criteria and practical implications
- **Where A2 outperforms A1**: Specific criteria and practical implications
- **Tie-breaking factors**: Context-specific considerations not captured by scored criteria (team familiarity, precedent in similar work, alignment with longer-term direction)

### 6. Validate Against Constraints

Before finalizing, verify the leading candidate satisfies all known constraints:

- **Resource constraints**: Required resources are available or obtainable
- **Time constraints**: Approach fits within timeline boundaries
- **Capability constraints**: Required skills or tools are accessible
- **Dependency constraints**: Prerequisites can be met before execution starts
- **Compatibility constraints**: Approach does not conflict with concurrent work

A candidate that scores highest but violates a hard constraint is ineligible. Promote the next-highest candidate and re-validate.

### 7. Document Selection

Record the decision with supporting evidence:

```markdown
## Selected Approach

**Approach**: [Description]
**Weighted Score**: [Score]
**Primary Rationale**: [Why this approach over alternatives]

### Trade-offs Accepted
- [Trade-off 1]: [Mitigation or acceptance rationale]
- [Trade-off 2]: [Mitigation or acceptance rationale]

### Alternatives Considered
| Approach | Score | Reason Not Selected |
|----------|-------|---------------------|
| [A2] | [Score] | [Specific reason] |
| [A3] | [Score] | [Specific reason] |
```

Include enough detail for a reviewer to understand the decision without re-running the evaluation.

## Decision Points

**Only one viable approach identified**: Skip scoring. Validate the single candidate against constraints (Step 6) and document it as the selected approach with a note that no alternatives existed.

**All candidates score below 3.0**: No approach meets minimum quality. Return to analysis for additional recommendations or reassess evaluation criteria -- the criteria may be unrealistic given available options.

**Tied scores after trade-off analysis**: Select the approach with lower risk score when effectiveness scores are equal. Lower-risk approaches preserve more options for course correction during execution.

**Criteria weights disputed**: Present both weight scenarios to stakeholders with resulting rankings. Show how weight changes affect the outcome to focus discussion on the specific trade-off rather than abstract priorities.

**New information surfaces during evaluation**: Re-score affected criteria for all candidates rather than adjusting only the newly informed candidate. Partial re-scoring introduces bias.

**Constraint violation discovered late**: Do not relax the constraint to preserve the selection. Eliminate the candidate and promote alternatives. Document the constraint that caused elimination for future reference.

## Success Criteria

- All candidate approaches extracted from analysis with evidence citations
- Evaluation criteria defined with weights summing to 1.0
- Every candidate scored on every criterion with documented rationale
- Trade-offs between leading candidates explicitly assessed
- Selected approach validated against all known constraints
- Decision documented with alternatives, scores, and rejection reasons

## Common Issues

**Anchoring on first candidate**: Evaluators score the first candidate, then compare others relative to it rather than against criteria independently. Score all candidates on one criterion before moving to the next criterion.

**Criteria selected to favor a preferred outcome**: Criteria should be established before examining candidates in detail. If criteria shift after scoring begins, restart with the revised criteria applied to all candidates equally.

**Missing rejection rationale**: Documenting only the selected approach obscures why alternatives were rejected. Future reassessment requires understanding what made alternatives inferior in the original context.

**Over-weighting reversibility**: High reversibility scores can elevate conservative approaches that never commit fully. Reversibility matters most when execution costs are high and outcomes are uncertain. For low-cost or well-understood changes, reduce its weight.

**Ignoring qualitative factors**: Weighted scoring captures quantifiable dimensions but misses team morale, organizational momentum, and learning value. Use trade-off analysis (Step 5) to surface these factors rather than forcing them into numeric scores.
