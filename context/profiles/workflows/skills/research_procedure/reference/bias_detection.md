# Bias Detection Procedure

Identify and mitigate bias in research sources to ensure balanced, evidence-based conclusions.

## Prerequisites

- Initial research results from external discovery
- Awareness of source types (vendor documentation, third-party comparisons, academic papers)
- Citation frequency data for sampled sources

## Steps

### 1. Categorize Sources by Type

Group sources into three categories:

- **Vendor materials**: Documentation, blogs, comparisons from tool creators
- **Third-party analysis**: Independent reviews, benchmarks, academic papers
- **Community content**: Forums, discussions, social media mentions

Vendor materials require scrutiny. Third-party sources need context validation. Community content needs triangulation.

### 2. Identify Vendor Bias

Examine comparisons and critiques for indicators:

- **Overstatement**: Claims of superiority without metrics
- **Strawman arguments**: Misrepresentation of alternative approaches
- **Omission**: Ignoring legitimate use cases for competing tools
- **Selective benchmarking**: Cherry-picked favorable comparisons

Distinguish opinion from empirical evidence. Opinion uses qualitative language ("better," "faster," "more intuitive"). Evidence provides specific metrics ("40% reduction in build time," "3x lower memory footprint").

### 3. Detect Selection Bias

Review citation distribution in research results. Popular sources receive disproportionate attention. A tool mentioned in 50 blog posts may appear superior to a tool mentioned in 5, even if the latter has stronger technical merit.

Sample across citation levels:
- **High-citation**: Sources appearing frequently in search results
- **Medium-citation**: Sources with moderate visibility
- **Low-citation**: Niche or emerging sources

Include at least one source from each tier when forming conclusions.

### 4. Cross-Validate Claims

Compare claims across source types. Vendor claims require third-party confirmation. Community opinions need empirical verification from formal sources.

Look for convergence:
- Independent sources confirm vendor benchmarks
- Multiple third parties reach similar conclusions
- Academic research aligns with practical observations

Contradictions require investigation. Determine whether differences stem from context, use cases, or actual bias.

## Decision Points

**Source inclusion**: Exclude sources with pure opinion and no evidence. Include sources with mixed content if empirical portions are identifiable and extractable.

**Weight assignment**: Assign lower weight to vendor claims without independent confirmation. Assign higher weight to peer-reviewed research with transparent methodology.

**Citation sampling**: When 80% of sources mention the same 3 tools, deliberately seek alternatives to avoid popularity bias.

## Success Criteria

- Vendor claims are identified and flagged
- Evidence sources span multiple citation levels
- Conclusions reference diverse source types
- Opinion and evidence are distinguished in outputs

## Common Issues

**Confirmation bias**: Research favors pre-existing preferences. Mitigation: Explicitly seek disconfirming evidence.

**Recency bias**: Over-weighting new sources. Mitigation: Include established sources even if older.

**Accessibility bias**: Favoring easily available sources. Mitigation: Search beyond the first page of results, use academic databases.

**Platform bias**: Sources concentrated on specific platforms (e.g., only GitHub stars, only Hacker News discussions). Mitigation: Cross-reference multiple platforms.
