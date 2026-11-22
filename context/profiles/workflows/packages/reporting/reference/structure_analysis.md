# Analysis Report Schema

Analysis reports interpret findings and generate actionable recommendations. They transform observations into insights, assess significance, and prioritize next steps. Analysis answers "what does it mean?" and "what should we do?"—building on discovery reports that answer "what exists?"

## Frontmatter

```yaml
---
type: analysis
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
focus: "{analytical question or domain}"
confidence: "{overall confidence level}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `analysis` |
| date | Yes | Report creation date |
| time | Yes | Report creation time |
| source | Yes | Work item context or "general" |
| objective | Yes | Objective that produced this report |
| focus | Yes | Central analytical question or domain |
| confidence | Yes | Overall confidence: High, Moderate, or Low |

## Required Sections

### Summary

2-4 sentences providing orientation. State:

- Analytical question addressed
- Primary insight or conclusion
- Key recommendation

Executives read only summaries—ensure standalone value.

### Input Sources

Reports and materials analyzed:

```markdown
## Input Sources

| Source | Type | Date | Key Topic |
|--------|------|------|-----------|
| discovery-config.md | discovery | {date_1} | Configuration inventory |
| validation-naming.md | validation | {date_2} | Naming compliance |
```

Include paths, types, and dates for all consumed materials.

### Key Insights

Major interpretive findings organized by theme. Each insight as H3:

```markdown
### {Insight Title}

{Statement of the insight}

- **Evidence**: {source attribution with location}
- **Confidence**: High | Moderate | Low
- **Implications**: {significance for decisions}
```

### Recommendations

Prioritized actions based on insights:

```markdown
### {Recommendation Title}

{Clear action statement}

- **Rationale**: {link to supporting insights}
- **Priority**: Critical | High | Medium | Low
- **Dependencies**: {prerequisites or related items}
```

Every recommendation requires supporting evidence from insights.

### Confidence Assessment

Overall confidence in conclusions:

- Summary of confidence level
- Factors supporting confidence
- Factors reducing confidence
- Information that would increase confidence

## Optional Sections

### Trade-offs

When recommending among alternatives:

| Option | Optimizes | Sacrifices |
|--------|-----------|------------|
| Option A | Speed | Flexibility |
| Option B | Flexibility | Complexity |

### Contradictions

Conflicts found across sources:

```markdown
### {Contradiction Description}

- **Source A**: {claim with attribution}
- **Source B**: {conflicting claim with attribution}
- **Resolution**: {rationale for chosen interpretation}
```

### Sensitivity Analysis

How conclusions change under different assumptions. Document key assumptions and their impact on recommendations.

### Next Steps

Immediate actions to advance recommendations, distinct from the recommendations themselves.

## Confidence Levels

| Level | Definition | Application |
|-------|------------|-------------|
| High | Strong evidence, consistent sources | Multiple sources agree, direct evidence |
| Moderate | Good evidence, minor gaps | Most sources agree, some indirect evidence |
| Low | Limited evidence, significant uncertainty | Few sources, conflicting evidence, inference |

State confidence per insight and overall. Document factors affecting certainty.

## Priority Levels

| Priority | Criteria |
|----------|----------|
| Critical | High impact, blocks progress, immediate action required |
| High | Significant impact, clear value, address soon |
| Medium | Moderate impact, worthwhile, can schedule |
| Low | Minor impact, address opportunistically |

## Example

```markdown
---
type: analysis
date: {date}
time: {time}
source: general
objective: analysis
focus: Configuration management improvement
confidence: Moderate
---

# Configuration Analysis Report

## Summary

Analyzed configuration discovery and validation findings. Primary insight: inconsistent naming conventions increase maintenance burden and error risk. Recommend standardizing naming before adding new configuration.

## Input Sources

| Source | Type | Date | Key Topic |
|--------|------|------|-----------|
| {time_1}-discovery-config.md | discovery | {date} | Configuration inventory |
| {time_2}-validation-naming.md | validation | {date} | Naming compliance |

## Key Insights

### Naming Inconsistency Creates Risk

Three naming patterns exist across 12 files, causing confusion during modifications.

- **Evidence**: `{time}-discovery-config.md:45-52`, `{time_2}-validation-naming.md:23-31`
- **Confidence**: High
- **Implications**: New team members require orientation; errors likely during updates

### Environment Handling Undocumented

No authoritative source defines environment variable precedence.

- **Evidence**: `{time}-discovery-config.md § Gaps`
- **Confidence**: Moderate
- **Implications**: Deployment behavior unpredictable across environments

## Recommendations

### Standardize Naming Convention

Adopt `{purpose}.{format}` pattern for all configuration files.

- **Rationale**: Addresses naming inconsistency insight
- **Priority**: High
- **Dependencies**: Team agreement on pattern

### Document Environment Precedence

Create reference documentation for configuration loading order.

- **Rationale**: Addresses undocumented handling insight
- **Priority**: Medium
- **Dependencies**: Investigation of initialization logic

## Confidence Assessment

Overall confidence: **Moderate**

Factors supporting: Multiple sources confirm naming issues, clear pattern evidence.

Factors reducing: Environment precedence based on gap identification, not direct observation.

Would increase confidence: Runtime testing of configuration loading.
```

## Length Guidelines

| Scope | Target Length |
|-------|---------------|
| Focused (single question) | 500-800 words |
| Standard (domain) | 800-1500 words |
| Comprehensive (multi-domain) | 1500-2000 words |

## Validation Checklist

- [ ] Frontmatter includes all required fields with overall confidence
- [ ] Summary provides standalone value in 2-4 sentences
- [ ] Input sources listed with paths, types, and dates
- [ ] Each insight includes evidence, confidence, and implications
- [ ] Each recommendation links to supporting insights
- [ ] Recommendations include priority and dependencies
- [ ] Confidence assessment documents supporting and reducing factors
- [ ] Word count within guidelines
