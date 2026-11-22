# Confidence Assessment Procedure

Assign confidence levels to synthesized findings using GRADE methodology. Each finding receives independent assessment based on evidence quality and consistency.

## Prerequisites

- Findings synthesized from evidence extraction
- Source quality tiers documented (academic, professional, informal)
- Evidence supporting each finding accessible for review

## Steps

### 1. Establish Baseline Confidence

Assign starting confidence based on source tier:

| Source Type | Starting Level |
|-------------|----------------|
| Peer-reviewed research | High |
| Professional/technical documentation | Moderate |
| Informal sources (blogs, forums) | Low |

Mixed-source findings start at the lowest tier present.

### 2. Apply Downgrading Factors

Reduce confidence when evidence exhibits limitations:

- **Risk of bias**: Conflicts of interest, selective reporting, methodological flaws
- **Inconsistency**: Contradictory results across sources without explanation
- **Indirectness**: Evidence addresses related but not exact question
- **Imprecision**: Wide confidence intervals, small sample sizes, vague conclusions

Each factor present reduces confidence by one level (High to Moderate, Moderate to Low).

### 3. Apply Upgrading Factors

Increase confidence when evidence shows strength:

- **Large effect size**: Consistent, substantial effects across sources
- **Dose-response relationship**: Clear correlation between variables

Upgrading factors apply only to findings starting at Moderate or higher. Very Low findings require additional evidence before upgrading.

### 4. Assess Qualitative Evidence (GRADE-CERQual)

For qualitative findings, evaluate four components:

- **Methodological limitations**: Rigor of data collection and analysis
- **Coherence**: Consistency of pattern across studies
- **Data adequacy**: Richness and quantity of supporting data
- **Relevance**: Applicability to the research question

Rate each component (no concerns, minor, moderate, serious). Aggregate concerns determine final confidence.

### 5. Document Confidence Rationale

Record the assessment for each finding:

```markdown
**Finding**: Framework X improves performance by 30-40%
**Confidence**: Moderate
**Rationale**: High baseline (peer-reviewed). Downgraded for imprecision (wide range). No upgrading factors present.
```

## Decision Points

**Conflicting upgrade/downgrade factors**: Apply downgrades first, then assess whether upgrades offset remaining concerns.

**Borderline cases**: When assessment falls between levels, favor the lower confidence and note the uncertainty.

**Insufficient evidence**: Mark as Very Low and flag for additional research rather than synthesizing weak conclusions.

## Success Criteria

- Every finding has assigned confidence level (High, Moderate, Low, Very Low)
- Baseline, downgrades, and upgrades documented for each finding
- Qualitative findings assessed using CERQual components
- Rationale recorded with specific factor citations

## Common Issues

**Anchoring on source prestige**: Evaluate evidence quality independently of publication venue. Peer-reviewed work can still have methodological flaws.

**Overlooking indirect evidence**: Flag findings that answer adjacent questions. Indirectness applies even when sources are high-quality.

**Aggregating without independence**: Assess each finding separately. High confidence on one finding does not transfer to related findings.
