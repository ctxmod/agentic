# Query Formulation

Decomposing broad research questions into specific, parallelizable queries maximizes source coverage and reduces search iterations.

## Core Procedure

Break complex research questions into 3-7 independent sub-queries. Each sub-query should be self-contained, specific enough to yield targeted results, and independent enough to execute in parallel.

### Decomposition Steps

1. **Identify the core question**: What specific answer is needed?
2. **Extract dimensions**: What aspects, perspectives, or facets does the question span?
3. **Formulate sub-queries**: One query per dimension, each independently searchable
4. **Add constraints**: Include domain, time period, methodology, or context filters per query
5. **Validate independence**: Confirm sub-queries can execute without depending on each other's results

## Query Specificity

Queries yield better results when they include domain context, specific terminology, and constraint filters.

| Specificity Level | Characteristics | Expected Quality |
|-------------------|-----------------|------------------|
| Vague | Broad terms, no constraints | Low—noisy, unfocused results |
| Moderate | Domain terms, partial constraints | Medium—relevant but scattered |
| Specific | Precise terminology, context filters, constraint qualifiers | High—targeted, actionable |

Include technology names, version numbers, constraint qualifiers, and desired content type in queries. Exclude topics explicitly when they would dominate results but are not relevant.

## Parallel Query Design

Design queries to cover complementary aspects without redundancy:

- **Methodological queries**: How was this done? What techniques exist?
- **Comparative queries**: How do approaches differ? What trade-offs exist?
- **Contextual queries**: What constraints, standards, or regulations apply?
- **Gap queries**: What remains unsolved? What limitations persist?

## Iterative Refinement

First-round queries identify the landscape. Subsequent rounds target gaps:

1. **Initial sweep**: Broad queries establishing baseline understanding
2. **Focused follow-up**: Narrower queries targeting identified gaps or contradictions
3. **Verification queries**: Confirming specific claims or checking for counter-evidence

Stop when new queries return information already captured. Diminishing returns signal adequate coverage.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/source_selection_academic.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure/reference/source_evaluation_academic.md" />
`ctx:github.com/ctxmod/writing_academic/context::shared::research@scholarly_prose/reference/broad_scan.md` (the scholarly broad-scan method, kept in the writing_academic module)
