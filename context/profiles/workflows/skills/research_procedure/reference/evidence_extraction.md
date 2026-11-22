# Evidence Extraction Procedure

Extract verifiable claims from research sources with proper attribution and cross-validation.

## Prerequisites

Research sources identified and prioritized via discovery procedures. Source material accessible for detailed review.

## Steps

### 1. Claim Documentation

Extract specific claims from source material. Attach source identifier to each claim during extraction:

- **Factual claims**: Verifiable statements about facts, events, or data
- **Analytical claims**: Interpretations, conclusions, or synthesis from authors
- **Methodological claims**: Statements about research methods or approaches

Record claims with their source identifiers in structured format for tracking.

### 2. Source Verification

Cross-reference claims across multiple independent sources:

| Confidence Level | Verification Requirement |
|------------------|--------------------------|
| High             | 3+ independent sources confirm |
| Medium           | 2 independent sources confirm |
| Low              | Single source only |

Prioritize sources from the last 12 months for fast-moving topics (technology, markets, recent research). Historical sources acceptable for stable foundational concepts.

### 3. Citation Assignment

Assign numbered inline citations to claims:

```markdown
The technique improves throughput by 40% [1].
```

Place citation immediately after the claim it supports. Use ascending numbers throughout document.

### 4. Metadata Collection

Compile complete source metadata for citations list:

```markdown
[1] Author Name. "Title of Source." Publication/Website, Year. URL. Accessed: YYYY-MM-DD
```

**Required fields**:
- Title (in quotes for articles, reports)
- Author or organization
- Publication venue or website
- Year of publication
- URL for web sources
- Access date for volatile web content (blogs, news, forums)

## Decision Points

### Conflicting Information

When sources disagree:

1. Flag claim with context: "Source A claims X, while Source B states Y"
2. Examine publication dates—prioritize more recent sources
3. Assess source authority and methodology
4. Note conflict explicitly rather than choosing one side
5. Present both perspectives with attribution

### Source Quality Assessment

Evaluate sources before relying on claims:

- **Academic**: Peer-reviewed journals, conference proceedings, preprints
- **Professional**: Industry reports, technical documentation, standards bodies
- **Informal**: Blog posts, forums, social media (require additional verification)

Lower-tier sources require corroboration from higher-tier sources.

## Success Criteria

- Every claim has traceable source attribution
- Conflicting information flagged with context
- Citations follow numbered format with complete metadata
- High-confidence claims verified across 3+ sources
- Fast-moving topics use sources from last 12 months

## Common Issues

### Over-Citation

Avoid citing every sentence. Group related claims under single citation when all derive from same source.

**Correct**:
> "The framework uses type inference [3], which reduces boilerplate code and improves type safety [3]."

**Incorrect**:
> "The framework uses type inference [3]. This reduces boilerplate code [3]. It also improves type safety [3]."

### Orphaned Citations

Every citation number in text must appear in metadata list. Every metadata entry must be referenced in text.

### Stale Sources

Re-verify time-sensitive claims if sources predate 12 months and topic evolves rapidly (software versions, market data, recent research).
