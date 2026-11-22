# Report Creation

Reports consolidate findings into standalone documents. Each report should be self-contained: readers understand it without consulting source materials or working files.

## Standalone Principle

Reports reference external sources through citations, not internal working files. This ensures:

- Reports remain valid when working files change or disappear
- Reports can be shared or referenced without context loss
- Claims are traceable to verifiable sources

**Correct**: "The system achieves 92% coverage for high-frequency codes [1]." with a Sources section citing the original study.

**Incorrect**: "See internal-reports/analysis-results.md for details." Internal working file references break when files move or are cleaned up.

## Report Structure

### Minimal Structure

Every report contains at minimum:

1. **Title**: Clear scope statement
2. **Overview**: 2-3 sentences establishing what the report covers and why
3. **Findings**: Organized by theme, not by source
4. **Sources**: Complete attribution for all cited material

### Extended Structure

For comprehensive reports:

1. **Title**
2. **Overview**
3. **Key Findings**: Summary of primary results
4. **Detailed Sections**: Organized thematically
5. **Limitations**: Boundaries of the analysis
6. **Related Reports**: Cross-references to complementary reports
7. **Sources**: Full citations

## Citation Practice

Use numbered citations throughout with a Sources section:

```markdown
The mapping achieves 88% coverage for high-frequency codes [1].
Foundation models trained on external data may not generalize to local contexts [2].

## Sources

[1] Author, A. et al. (Year). Title. URL or DOI
[2] Author, B. et al. (Year). Title. URL or DOI
```

Include sufficient metadata for source verification: author, year, title, and a stable identifier (DOI, URL).

## First-Hand Attribution

For information from meetings, interviews, or direct observation, attribute explicitly:

```markdown
Per [Name] ([context], [date]):
> [Statement or close paraphrase]
```

This pattern applies to meeting notes, stakeholder input, field observations, or any non-published source.

## Thematic Organization

Organize content by theme, not by source or chronology:

**Source-organized** (avoid): "Paper A says X. Paper B says Y. Paper C says Z."

**Theme-organized** (target): "Three patterns emerge across the literature: [theme 1], [theme 2], [theme 3]. Each draws support from multiple independent sources."

Thematic organization demonstrates synthesis rather than collection.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::writing_procedure/reference/academic_organization.md" />
`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/evidence_extraction_academic.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
