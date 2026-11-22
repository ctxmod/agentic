# Impact Scoring Procedure

Assess gaps and issues across three dimensions: business value, risk severity, and frequency. Combine scores to prioritize remediation efforts.

## Prerequisites

- Gap or issue clearly defined with observable symptoms
- Stakeholder input available for business context
- Access to operational data (logs, metrics, incident history)

## Steps

### 1. Score Business Value (1-5)

| Score | Definition | Examples |
|-------|------------|----------|
| 5 | Revenue-critical | Payment processing, checkout flow |
| 4 | Compliance-required | Data retention, audit logging |
| 3 | Core user journey | Account management, search |
| 2 | Secondary feature | Notifications, preferences |
| 1 | Nice-to-have | Cosmetic improvements |

### 2. Score Risk Severity (1-5)

| Score | Definition | Financial Threshold |
|-------|------------|---------------------|
| 5 | Catastrophic | >$1M exposure, security breach |
| 4 | Major | $100K-$1M, data loss potential |
| 3 | Moderate | $10K-$100K, service degradation |
| 2 | Minor | $1K-$10K, user inconvenience |
| 1 | Negligible | <$1K, cosmetic defect |

### 3. Score Frequency (1-5)

| Score | Definition | Occurrence |
|-------|------------|------------|
| 5 | Constant | Multiple times daily |
| 4 | Frequent | Daily |
| 3 | Regular | Weekly |
| 2 | Occasional | Monthly |
| 1 | Rare | Quarterly or less |

### 4. Calculate Weighted Severity

Apply weights based on organizational priorities:

```
Weighted Score = (Value × 0.4) + (Risk × 0.4) + (Frequency × 0.2)
```

Adjust weights for context. Security-focused organizations may use Risk × 0.5.

### 5. Plot on Probability-Impact Matrix

```
         Risk-Value Score
         1    2    3    4    5
    5  | M  | M  | H  | H  | C  |
F   4  | L  | M  | M  | H  | H  |
r   3  | L  | L  | M  | M  | H  |
e   2  | L  | L  | L  | M  | M  |
q   1  | L  | L  | L  | L  | M  |

C=Critical, H=High, M=Medium, L=Low
```

## Decision Points

**Scores cluster near boundaries**: Round toward higher priority when scores fall between 2.5-3.5. Err on caution for security-related gaps.

**Stakeholders disagree on value**: Use maximum proposed value score. Document dissenting perspectives.

**No frequency data available**: Default to score 3 (weekly) and flag for monitoring to gather actual data.

**Multiple affected systems**: Score each system independently. Use highest individual score for prioritization.

## Success Criteria

- All three dimensions scored with rationale documented
- Weighted score calculated using agreed weights
- Gap plotted on probability-impact matrix
- Priority level assigned (Critical/High/Medium/Low)

## Common Issues

**Inflated business value**: Require concrete revenue or compliance linkage. "Important" without evidence defaults to score 2.

**Underestimated frequency**: Cross-reference user reports with logs. Silent failures often occur more than reported.

**Missing financial thresholds**: Establish organizational defaults before scoring. Use industry benchmarks when internal data unavailable.
