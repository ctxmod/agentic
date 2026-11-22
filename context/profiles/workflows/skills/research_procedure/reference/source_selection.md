# Source Selection Procedure

## Prerequisites

- Research question clearly articulated
- Question type classified (best practices, academic foundations, implementation patterns, technology comparison)
- Time constraints established for research phase

## Steps

1. **Classify the research question** by primary objective:
   - **Best practices**: Current industry standards, conventions, recommended approaches
   - **Academic foundations**: Peer-reviewed research, theoretical frameworks, scholarly consensus
   - **Implementation patterns**: Code examples, library usage, practical application
   - **Technology comparison**: Framework evaluation, tool selection, trade-off analysis

2. **Select primary sources** using the decision matrix:

| Question Type | Primary Sources | Tool |
|---------------|-----------------|------|
| Best practices | Technical blogs, documentation | `perplexity_search` |
| Academic foundations | Peer-reviewed papers, arXiv | `academia_search` |
| Implementation patterns | GitHub, Stack Overflow | `perplexity_search` |
| Technology comparison | Industry analysis, benchmarks | `perplexity_deep_research` |

3. **Match tool to scope**:
   - **Quick lookups**: Use `perplexity_search` (30-60 seconds, 20 sources) for narrow questions
   - **Comprehensive analysis**: Use `perplexity_deep_research` (5-10 minutes, 100 sources) for complex, multi-faceted topics

4. **Construct query** with 8-15 words including:
   - Technology names and versions
   - Specific concept or technique
   - Application context
   - Desired outcome or comparison dimension
   - Temporal constraints for fast-moving topics (e.g., "2026")

5. **Execute tool** with selected parameters. Monitor response time against established scope.

## Decision Points

**Mixed question types**: Break compound questions into sub-questions. Address each with appropriate tool. Example: "Best practices for implementing X" combines best practices (`perplexity_search`) + implementation patterns (`perplexity_search`).

**Time-critical research**: Use `perplexity_search` for rapid iteration. Reserve `perplexity_deep_research` for decision points requiring comprehensive analysis.

**Academic depth needed**: Start with `academia_search` for foundational papers. Follow with `perplexity_search` for current industry application.

**Conflicting sources**: Verify claims across 3+ independent sources. Prioritize sources from last 12 months for fast-moving topics. Flag disagreements with context in reports.

**Insufficient results**: Expand query by removing specificity (fewer technical terms), adding synonyms, or using query expansion features when available.

## Success Criteria

- Question type maps to appropriate tool and source type
- Query contains sufficient technical specificity without being overly narrow
- Tool executes within expected time bounds
- Results include sources from appropriate quality tiers
- Research output provides actionable evidence for objectives

## Common Issues

**Source type mismatch**: Using academic search for implementation patterns yields theoretical papers without code examples. Align source type to question needs.

**Overly specific queries**: "TypeScript discriminated unions error handling async functions Result type either pattern 2026" is too narrow. Remove temporal or niche modifiers to expand results.

**Under-specified queries**: "Error handling" returns generic advice. Add technology, context, and specific problem dimensions.

**Tool scope confusion**: Using `perplexity_deep_research` for simple lookups wastes time. Use 20-source tools for targeted questions.

**Ignoring temporal relevance**: Academic sources from 2018 may precede major framework changes. Prioritize recent sources for fast-moving technologies.
