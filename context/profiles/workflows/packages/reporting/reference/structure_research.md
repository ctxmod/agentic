# Research Report Schema

Structure and conventions for research reports documenting external evidence with source quality tiering and thematic organization

Research reports document external evidence gathered through systematic inquiry. They synthesize findings from web sources, academic literature, documentation, and community discussions into thematic groupings with source quality assessment. Research answers "what does external evidence say?"--distinct from discovery ("what exists internally?") and analysis ("what does it mean?").

## Frontmatter

```yaml
---
type: research
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
scope: "{research scope description}"
queries:
  - "{search query 1}"
  - "{search query 2}"
tools:
  - "{tool or database used}"
date_range: "{YYYY-MM to YYYY-MM or 'current'}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `research` |
| date | Yes | Report creation date |
| time | Yes | Report creation time |
| source | Yes | Work item context or "general" |
| objective | Yes | Objective that produced this report |
| scope | Yes | Brief description of research scope |
| queries | Yes | Search queries executed (array) |
| tools | Yes | Research tools or databases accessed (array) |
| date_range | No | Publication date range of sources examined |

Research frontmatter captures search metadata for reproducibility. Discovery uses location categories; analysis uses confidence levels; research uses queries and tools.

## Required Sections

### Summary

1-3 sentences providing orientation. State:

- Primary findings and their significance
- Number of sources examined and quality distribution
- Areas of consensus or notable disagreement

The summary enables consumers to decide whether the full report warrants reading.

### Sources

All sources organized by quality tier:

```markdown
## Sources

### Tier 1: Authoritative

Official documentation, peer-reviewed research, and primary sources.

- [Product: Page Title](URL) (accessed YYYY-MM-DD)
- [Paper Title](URL) (accessed YYYY-MM-DD)

### Tier 2: Established

Respected technical publications, well-maintained guides, and expert analyses.

- [Author/Publication: Title](URL) (accessed YYYY-MM-DD)

### Tier 3: Community

Forum discussions, blog posts, and informal references.

- [Platform: Discussion Title](URL) (accessed YYYY-MM-DD)
```

Position the Sources section early so readers can assess evidence quality before encountering findings. Every source listed here must appear in at least one finding.

### Findings by Theme

Primary content organized by theme, not by individual source. Each theme synthesizes evidence from multiple sources:

```markdown
## Findings by Theme

### {Theme Title}

{Synthesis of what sources collectively indicate about this theme}

- **Evidence**: [Source 1](URL), [Source 2](URL)
- **Consensus**: {degree of agreement across sources}
- **Practical implication**: {what this means for downstream work}
```

Target 3-7 themes per report. Themes emerge from patterns across sources rather than mapping one-to-one with individual sources. A source may contribute to multiple themes.

### Gaps

Areas where evidence is missing, insufficient, or inconclusive:

- What the research could not determine
- Why the gap exists (limited sources, emerging topic, conflicting evidence)
- Suggested approaches to address the gap

Gaps inform downstream analysis about confidence boundaries and areas requiring further investigation.

### Metadata

Search execution details for reproducibility:

- **Queries executed**: Full queries with tool context
- **Tools accessed**: Search engines, databases, APIs used
- **Total sources examined**: Count including sources not cited
- **Date range**: Publication dates of examined sources
- **Limitations**: Constraints affecting coverage (access restrictions, language barriers, recency)

## Optional Sections

### Consensus and Controversy

Include when sources present conflicting positions on the same topic:

```markdown
### {Topic of Disagreement}

- **Position A**: {claim with source attribution}
- **Position B**: {opposing claim with source attribution}
- **Assessment**: {evaluation of which position has stronger evidence and why}
```

Omit this section when sources broadly agree. Integrate minor disagreements into the Findings by Theme section instead.

### Recommendations

Include when research findings directly inform implementation or design decisions:

```markdown
### {Recommendation}

- **Based on**: {theme or finding supporting this recommendation}
- **Confidence**: High | Moderate | Low
- **Caveat**: {conditions or limitations}
```

Recommendations in research reports are evidence-based suggestions, not prioritized action plans. Analysis reports handle prioritization and trade-off evaluation.

### Anti-Patterns

Include when research reveals common mistakes, deprecated approaches, or patterns to avoid:

```markdown
### {Anti-Pattern Name}

- **Description**: {what the anti-pattern involves}
- **Evidence**: {sources documenting problems with this approach}
- **Alternative**: {preferred approach based on research}
```

## Source Quality Tiers

| Tier | Description | Examples |
|------|-------------|----------|
| Tier 1: Authoritative | Official, peer-reviewed, or primary sources | Official docs, RFCs, peer-reviewed papers, language specs |
| Tier 2: Established | Respected secondary sources with editorial oversight | Technical publications, maintained guides, conference talks |
| Tier 3: Community | Informal sources without editorial review | Blog posts, forum answers, social media discussions |

Tier assignment reflects source reliability, not content quality. A well-written blog post remains Tier 3; a poorly organized RFC remains Tier 1. When a finding relies solely on Tier 3 sources, flag reduced confidence in the finding.

## Attribution Format

Research attribution emphasizes source quality and temporal validity rather than file locations:

| Source Type | Format |
|-------------|--------|
| Documentation | `[Product: Page](URL) (Tier 1, accessed YYYY-MM-DD)` |
| Academic paper | `[Title](URL) (Tier 1, accessed YYYY-MM-DD)` |
| Technical article | `[Author: Title](URL) (Tier 2, accessed YYYY-MM-DD)` |
| Community source | `[Platform: Title](URL) (Tier 3, accessed YYYY-MM-DD)` |
| Observation | `[observation: context]` |

Access dates establish temporal validity. Research findings may become outdated as tools, frameworks, and best practices evolve.

## Structural Differences from Other Schemas

| Aspect | Discovery | Research | Analysis |
|--------|-----------|----------|----------|
| Domain | Internal artifacts | External sources | Combined findings |
| Organization | By category | By theme | By insight |
| Attribution | File paths, line ranges | URLs, access dates, quality tiers | Source reports with dates |
| Quality signal | Location specificity | Source tier assessment | Confidence per insight |
| Unknowns | Gaps in coverage | Gaps + consensus/controversy | Confidence assessment |
| Metadata | Sources, method | Queries, tools, date range | Input sources table |

## Example

```markdown
---
type: research
date: {date}
time: {time}
source: general
objective: research
scope: Error handling patterns in async Python
queries:
  - "Python asyncio exception handling patterns TaskGroup ExceptionGroup"
  - "structured concurrency error propagation Python"
tools:
  - Perplexity Search
  - Google Scholar
date_range: "2023-01 to current"
---

# Async Error Handling Research

## Summary

Examined 14 sources on async error handling in Python 3.11+. TaskGroup and ExceptionGroup form the foundation of structured concurrency error handling, with broad consensus on their adoption. Source quality is strong: 4 Tier 1, 6 Tier 2, 4 Tier 3.

## Sources

### Tier 1: Authoritative

- [Python: asyncio Task Groups](https://docs.python.org/3/library/asyncio-task.html) (accessed {date})
- [PEP 654: Exception Groups](https://peps.python.org/pep-0654/) (accessed {date})
- [PEP 678: Enriching Exceptions with Notes](https://peps.python.org/pep-0678/) (accessed {date})
- [Guido van Rossum: Reasoning about asyncio.Semaphore](https://example.com) (accessed {date})

### Tier 2: Established

- [Hynek Schlawack: Structured Concurrency in Python](https://example.com) (accessed {date})
- [Real Python: Async IO in Python](https://example.com) (accessed {date})

### Tier 3: Community

- [StackOverflow: TaskGroup exception handling](https://example.com) (accessed {date})
- [Reddit r/Python: ExceptionGroup patterns](https://example.com) (accessed {date})

## Findings by Theme

### TaskGroup as Primary Error Boundary

TaskGroup cancels sibling tasks when any task raises, then collects exceptions into an ExceptionGroup. This replaces manual gather-and-filter patterns from earlier Python versions.

- **Evidence**: PEP 654, Python asyncio docs, Hynek Schlawack article
- **Consensus**: Strong across all source tiers
- **Practical implication**: Design task groups around failure domains where sibling cancellation is acceptable

### ExceptionGroup Handling with except*

The except* syntax enables selective handling of exception types within a group without discarding unhandled exceptions. Multiple except* clauses can match different exception types from the same group.

- **Evidence**: PEP 654, Python docs, Real Python guide
- **Consensus**: Strong on syntax; moderate on best practices (still emerging)
- **Practical implication**: Use except* for granular error handling; avoid bare except on ExceptionGroups

### Graceful Shutdown Patterns

Cancellation during shutdown requires timeout-bounded cleanup. Sources agree on a two-phase pattern: request cancellation, then enforce timeout.

- **Evidence**: Python docs, Hynek Schlawack, 2 StackOverflow discussions
- **Consensus**: Moderate--timeout values and cleanup ordering vary across sources
- **Practical implication**: Implement shield() for critical cleanup; set explicit cancellation timeouts

## Gaps

- Limited coverage of ExceptionGroup patterns in production systems at scale
- No authoritative guidance on exception group nesting depth limits
- Testing patterns for TaskGroup error scenarios poorly documented across all tiers

## Consensus and Controversy

### Bare asyncio.gather vs TaskGroup

- **Position A**: TaskGroup replaces gather entirely for Python 3.11+ (Hynek Schlawack, multiple community sources)
- **Position B**: gather remains appropriate for fire-and-forget scenarios where cancellation is undesirable (2 StackOverflow answers)
- **Assessment**: Position A has stronger evidence. TaskGroup provides safer defaults. gather remains valid only when explicit return_exceptions=True behavior is needed.

## Metadata

- **Queries executed**: 2 primary queries plus 3 follow-up refinements
- **Tools accessed**: Perplexity Search, Google Scholar
- **Total sources examined**: 22 (14 cited, 8 excluded for redundancy or low relevance)
- **Date range**: January 2023 to current
- **Limitations**: Python 3.11+ focus excludes pre-TaskGroup patterns; English-language sources only
```

## Length Guidelines

| Scope | Target Length |
|-------|---------------|
| Focused (single question) | 500-800 words |
| Standard (domain) | 800-1500 words |
| Comprehensive (multi-domain) | 1500-2000 words |

## Validation Checklist

- [ ] Frontmatter includes all required fields with queries and tools
- [ ] Summary states findings, source count, and consensus level in 1-3 sentences
- [ ] Sources organized by quality tier with URLs and access dates
- [ ] Findings organized by theme (3-7 themes), not source-by-source
- [ ] Every finding cites sources; every source appears in at least one finding
- [ ] Gaps document missing evidence with suggested approaches
- [ ] Metadata records queries, tools, total sources, and limitations
- [ ] Source tier assignments reflect reliability, not content quality
- [ ] Word count within guidelines

## Related

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_discovery.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_analysis.md" />
- `reference/procedures/research/query-formulation.md`
- `reference/procedures/research/source-evaluation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
