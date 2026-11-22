# Criteria Weighting Procedure

Construct decision matrices that systematically compare alternatives using weighted evaluation criteria. This procedure produces ranked outcomes with transparent scoring logic.

## Prerequisites

- Alternatives identified from research or discovery (3-10 candidates)
- Research context established with domain constraints
- Evaluation purpose defined (selection, ranking, or elimination)

## Steps

### 1. Define Evaluation Criteria

Select 3-7 criteria that discriminate meaningfully between alternatives. Draw criteria from:

- **Research findings**: Performance benchmarks, capability comparisons
- **Constraints**: Budget limits, timeline requirements, compatibility needs
- **Stakeholder priorities**: User experience, maintainability, scalability

Name criteria precisely: "Query latency under 50ms" rather than "performance."

### 2. Assign Criterion Weights

Choose one weighting approach:

**Direct Assignment (SMART)**: Assign importance scores 1-100, then normalize to sum to 1.0.

| Criterion | Raw Score | Weight |
|-----------|-----------|--------|
| Latency   | 40        | 0.40   |
| Cost      | 35        | 0.35   |
| Ease of migration | 25 | 0.25  |
| **Total** | 100       | 1.00   |

**Pairwise Comparison (AHP)**: Compare each criterion pair: "Is latency more important than cost?" Score pairs 1-9, build comparison matrix, calculate eigenvector for weights.

Use direct assignment for 3-4 criteria or time-constrained decisions. Use pairwise comparison for 5+ criteria or contested priorities.

### 3. Score Alternatives

Rate each alternative 0.0-1.0 on each criterion:

- **1.0**: Fully satisfies criterion
- **0.5**: Partially satisfies criterion
- **0.0**: Fails criterion

Normalize quantitative data to 0-1 scale. For latency where lower is better: `score = 1 - (value / max_value)`.

### 4. Calculate Weighted Totals

Multiply each score by its criterion weight, sum across criteria:

```
Total = (weight_1 × score_1) + (weight_2 × score_2) + ... + (weight_n × score_n)
```

Rank alternatives by total score descending.

### 5. Perform Sensitivity Analysis

Test ranking stability by adjusting top criterion weight +/-20%. If rankings change, document threshold where flip occurs and report finding to stakeholders.

## Decision Points

**Criteria exceed 7**: Merge related criteria or drop least discriminating. Matrices with 8+ criteria produce noisy results.

**Tie scores within 0.05**: Report tie explicitly. Examine qualitative factors or gather additional data to break tie.

**Single criterion dominates**: If one weight exceeds 0.5, validate that dominance reflects actual priorities.

## Success Criteria

- Weights sum to 1.0 exactly
- All alternatives scored on all criteria
- Rankings calculated and documented
- Sensitivity analysis completed for top-weighted criterion
- Decision rationale traceable through matrix

## Common Issues

**Criteria overlap**: "Performance" and "speed" measure same attribute. Merge or redefine to ensure independence.

**Score clustering**: All alternatives score 0.7-0.8 on a criterion. Criterion fails to discriminate; remove or refine measurement scale.

**Weight inflation**: Assigning 0.6+ to one criterion often signals missing criteria. Decompose the dominant criterion into components.

**Quantitative/qualitative mixing**: Convert qualitative assessments to scores before matrix entry. Document conversion rationale.
