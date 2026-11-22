# Source Evaluation Procedure

Assess the credibility and quality of research sources to determine trustworthiness and weight in decision-making. This procedure applies to web sources, academic papers, documentation, and community content.

## Prerequisites

- Source identified through external discovery
- Research question or claim requiring verification
- Access to source metadata (author, date, publisher)

## Steps

1. **Assess credibility dimensions** across five criteria:

   - **Authority**: Domain reputation, author credentials, institutional affiliation. Peer-reviewed publications, official documentation, and recognized experts score highest.
   - **Recency**: Publication date relative to topic velocity. Fast-moving topics (frameworks, tools) require sources from last 12 months. Stable topics (algorithms, theory) tolerate older sources.
   - **Citations**: Academic citation count for papers, community references for practical content. High citations indicate influence but guard against popularity bias.
   - **Consensus**: Agreement with other credible sources. Cross-check claims against 3+ independent sources. Note conflicts with context.
   - **Transparency**: Methodology disclosure, reproducibility, conflict-of-interest statements. Transparent sources allow verification.

2. **Classify quality tier** using source characteristics:

   | Tier | Description | Indicators |
   |------|-------------|------------|
   | **1: Authoritative** | Peer-reviewed, official documentation, standards | Academic journals, RFCs, official docs, ARXIV with citations |
   | **2: Reputable** | Established expertise, technical leadership | Conference talks, technical blogs from known experts, reputable publishers |
   | **3: Community** | Unverified collective wisdom | Forums, discussions, Stack Overflow, personal blogs without credentials |

3. **Detect bias** by examining source context. Vendor-sponsored comparisons favor their products. Opinion pieces present unverified claims as fact. Sample diverse citation levels to avoid over-representing popular sources.

4. **Assign confidence score** based on tier and dimensions:
   - Tier 1: High confidence (authoritative, recent, transparent)
   - Tier 2: Medium-high confidence (reputable, recent)
   - Tier 3: Medium confidence (use for validation, not primary claims)

5. **Document assessment** with source metadata, tier classification, and rationale. Flag sources requiring additional verification.

## Decision Points

**Conflicting sources across tiers**: Trust Tier 1-2 over Tier 3. When Tier 1 sources disagree, investigate methodology differences and publication dates.

**Outdated but authoritative source**: Check if cited by recent sources. If still referenced, foundational insights remain valid. If uncited in recent work, likely superseded.

**Single source for claim**: Treat as unverified. Require 3+ independent sources before accepting as consensus. Note source gaps in report.

**Vendor documentation vs community reports**: Vendor docs provide authoritative implementation details. Community reports reveal real-world issues and limitations.

## Success Criteria

- Every source assessed against all five credibility dimensions
- Quality tier assigned with documented rationale
- Biases and conflicts identified
- Confidence score determined
- Sources flagged for additional verification when needed

## Common Issues

**Popularity bias**: Highly-cited or frequently linked sources may reflect marketing, not quality. Evaluate methodology, not just metrics.

**Recency fallacy**: Assuming new sources are always better. Foundational concepts may have stable, authoritative explanations from older sources.

**Authority overreach**: Expert in one domain speaking outside their expertise. Verify credentials match topic area.

**Transparency gaps**: Sources without methodology or attribution cannot verify claims. Use only for directional insights, not definitive conclusions.

**Consensus false positives**: Multiple sources repeating same unverified claim (common in documentation). Trace claims to primary sources.
