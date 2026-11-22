# Pattern Aggregation Procedure

Identify recurring patterns across multiple research sources through systematic coding and framework synthesis. This procedure transforms scattered findings into integrated conceptual structures.

## Prerequisites

- Extracted findings from 3+ independent sources with source attribution
- Initial conceptual schema or framework relevant to research objective
- Source identifiers attached to each finding for traceability

## Steps

### 1. Establish Initial Framework

Start with a preliminary schema based on research objectives. Define 5-8 high-level categories that reflect expected themes:

```
Categories: [Performance, Reliability, Maintainability, Security, Scalability]
```

This framework provides structure while remaining open to extension.

### 2. Code Individual Findings

Assign each finding to framework categories. Record source identifier with each coding decision:

| Finding | Category | Source |
|---------|----------|--------|
| "Caching reduces latency by 60%" | Performance | S1, S3 |
| "Circuit breakers prevent cascading failures" | Reliability | S2, S4 |

When findings span multiple categories, assign to all relevant categories.

### 3. Identify Frequency Patterns

Mark findings appearing in 2+ sources as potential patterns. Calculate frequency:

- **Strong pattern**: 4+ sources (>60% coverage)
- **Moderate pattern**: 2-3 sources (30-60% coverage)
- **Weak signal**: 1 source only (<30% coverage)

Prioritize strong and moderate patterns for framework integration.

### 4. Build Concept-to-Source Matrix

Create a matrix mapping concepts to their supporting sources:

| Concept | S1 | S2 | S3 | S4 | S5 | Count |
|---------|----|----|----|----|----|----|
| Caching strategies | X | | X | X | | 3 |
| Circuit breakers | | X | | X | X | 3 |
| Rate limiting | X | X | X | X | X | 5 |

This matrix enables triangulation and identifies which concepts have strongest cross-source support.

### 5. Extend Framework

Add new categories when findings cluster outside existing schema. Extend only when 3+ findings share a theme not captured by current categories.

### 6. Synthesize Analytical Themes

Move from descriptive themes (what sources say) to analytical themes (what the patterns mean). Group related patterns into higher-order concepts that explain relationships.

## Decision Points

**Finding fits multiple categories equally**: Assign to primary category based on author emphasis. Note secondary categories in metadata.

**New theme emerges mid-analysis**: Create provisional category. Validate by checking if 3+ findings cluster there before finalizing.

**Sources contradict on pattern**: Document both perspectives with source attribution. Note disagreement as a finding itself.

**Framework becomes unwieldy (>12 categories)**: Consolidate related categories or introduce hierarchy with parent-child relationships.

## Success Criteria

- All findings coded to at least one category
- Frequency calculated for each pattern
- Concept-to-source matrix complete
- Framework extended only when evidence supports new categories
- Analytical themes synthesized from descriptive patterns

## Common Issues

**Premature framework extension**: Resist adding categories for single findings. Wait for clustering evidence from multiple sources.

**Over-reliance on frequency**: High-frequency patterns are not always most important. Evaluate significance alongside count.

**Losing source attribution**: Maintain source identifiers through all coding stages. Attribution enables verification and triangulation.

**Conflating descriptive and analytical themes**: Keep these stages separate. First capture what sources report, then interpret meaning across sources.
