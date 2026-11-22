# Report Organization

Consistent organization enables navigation, prevents duplication, and supports long-running processes where outputs accumulate.

## Directory Principles

Organize outputs by content type, then by topic:

```
{output-directory}/
├── {type}/
│   └── {category}/
│       └── {topic}.md
```

**Content types** group outputs by function:

- **Summaries**: Distilled sources with citations
- **Analyses**: Critical evaluation of single or multiple sources
- **Syntheses**: Integration connecting multiple findings
- **Concepts**: Atomic idea notes (one concept per file)

## Atomic Note Principle

Each output file should focus on one idea or topic. Atomic organization enables:

- **Reuse**: Individual findings referenced from multiple contexts
- **Independent evaluation**: Each claim assessable on its own merits
- **Flexible restructuring**: Notes rearranged without breaking compound documents
- **Clear attribution**: Each file traces to specific sources

**Too broad** (hard to reuse): A single file covering architecture, performance, limitations, and future directions of a topic.

**Atomic** (easy to link): Separate files for architecture overview, performance benchmarks, known limitations, and research directions.

## Cross-Referencing

Link related outputs using relative paths:

```markdown
See [related analysis](../analyses/topic-comparison.md)
For methodology details, see [methods overview](../methods/systematic-review.md)
```

Relative paths survive directory moves better than absolute paths. Link between related outputs to build navigable knowledge structures.

## Index Files

Create index files in directories containing more than five outputs:

```markdown
# {Directory Topic}

## Overview
Brief description of directory contents.

## Contents
- [Output A](path/to/a.md) - Brief description
- [Output B](path/to/b.md) - Brief description
```

Index files provide navigation entry points and serve as content inventories for maintenance.

## Naming Conventions

File names should be descriptive and consistent within a directory:

- Use lowercase with hyphens: `topic-subtopic.md`
- Include dates when chronological ordering matters: `2026-01-28-meeting-notes.md`
- Use numbered prefixes for ordered content: `01-overview.md`, `02-methodology.md`

## Maintenance

Periodic maintenance prevents organization decay:

- **Remove stale outputs**: Archive or delete superseded content
- **Update cross-references**: Fix broken links after restructuring
- **Consolidate**: Merge small files that have grown into a single theme
- **Split**: Break large files that have accumulated multiple topics

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::writing_procedure/reference/academic_creation.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
