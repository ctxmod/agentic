# External Discovery Objective

External discovery gathers information from sources outside the codebase: web search, academic databases, technical documentation, and community discussions. This variant bridges knowledge gaps between agent training cutoffs and current practices.

## Base Behavior

the `discovery_objective` skill

## Methodology

### Query Formulation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Formulate effective search queries for the external research topic.
`ctx:github.com/ctxmod/agentic/context::shared::research_procedure@workflows/reference/query_formulation.md`
```

### Source Selection

Brief template for a procedure-depth agent (launched from objective depth):

```text
Select appropriate sources and databases for the research queries.
`ctx:github.com/ctxmod/agentic/context::shared::research_procedure@workflows/reference/source_selection.md`
```

### Information Extraction

Brief template for a procedure-depth agent (launched from objective depth):

```text
Extract evidence and key findings from the selected sources.
`ctx:github.com/ctxmod/agentic/context::shared::research_procedure@workflows/reference/evidence_extraction.md`
```

### Source Evaluation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Evaluate the credibility and quality of each source.
`ctx:github.com/ctxmod/agentic/context::shared::research_procedure@workflows/reference/source_evaluation.md`
```

### Bias Detection

Brief template for a procedure-depth agent (launched from objective depth):

```text
Detect and account for bias across the gathered sources.
`ctx:github.com/ctxmod/agentic/context::shared::research_procedure@workflows/reference/bias_detection.md`
```

## Tool Permissions

**Allowed**: `perplexity_search`, `academia_search`, `WebSearch`, `WebFetch`, Write (reserved report path only)

**Forbidden**: Modifying source files, making implementation decisions, launching sub-agents

## Report Structure

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the external discovery report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

External discovery reports include these objective-specific sections:

| Section | Content |
|---------|---------|
| Sources | Categorized by tier with URLs |
| Findings by Theme | Organized by topic, not by source |
| Consensus and Controversy | Areas of agreement vs disagreement across sources |
| Related | Cross-references to existing codebase files |

### Citation Standards

Use numbered inline citations (`[1]`, `[2]`) placed after claims. Include complete source metadata in footer: title, author, year, URL. Add access dates for volatile web content.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Single-source claims | Bias risk, unverified | Verify across 3+ sources |
| Ignoring source date | Outdated information | Prioritize last 12 months |
| Conflicting sources without context | Confuses decisions | "Sources disagree on X because Y" |
| Citation overload | Reduces readability | Cite representative sources |
| Returning research inline | Bloats context | Write report, return summary + path |

## Examples

**Best Practices**: Query "TypeScript discriminated unions error handling 2026" via `perplexity_search`. Output: 4 Tier-2 sources with Result pattern consensus.

**Academic**: Query "graph neural networks molecular property prediction" via `academia_search`. Output: 8 peer-reviewed papers sorted by citations.

**Comparison**: Query "PostgreSQL vs MongoDB vector search 2026" via `perplexity_deep_research`. Output: 15 sources across tiers with trade-off matrix.

## Related

`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
