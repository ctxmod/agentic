# Research Note Templates

Structured templates for paper summaries, concept notes, and meeting notes

Research notes follow typed templates to ensure consistent structure and enable cross-referencing. Three core types serve distinct purposes: source notes capture literature, concept notes synthesize ideas, and structure notes organize connections.

## Note Type Taxonomy

| Type | Purpose | Granularity | Example |
|------|---------|-------------|---------|
| Source note (paper summary) | Capture one source's content and assessment | One per source | Summary of a journal article |
| Concept note | Synthesize one idea across sources | One idea per note | Definition and evidence for a concept |
| Structure note (hub note) | Connect related notes into navigable clusters | One per theme or topic | Overview linking related concept notes |

The atomic principle governs all types: one main focus per note. Atomic notes enable reuse across contexts, independent evaluation of individual claims, and flexible reorganization without breaking compound documents.

## Paper Summary Template

### Standard Template

Use for sources that warrant full analysis.

```markdown
# [Paper Title]

**Citation**: [Full citation in chosen style]

## Summary
[2-3 sentence overview of the paper's contribution]

## Key Findings
- Finding 1 (p. X)
- Finding 2 (p. Y)
- Finding 3 (p. Z)

## Methodology
[Brief description of research design, sample, and analysis approach]

## Critical Assessment
- **Strengths**: [Methodological rigor, theoretical contribution, or practical value]
- **Limitations**: [Scope constraints, generalizability concerns, or methodological gaps]

## Connection to Research
[How this source relates to current research questions or objectives]

## Sources
[1] Author, A. et al. (Year). Title. DOI or URL
```

### Quick Screening Template

Use during initial literature screening to triage sources before full analysis.

```markdown
# [Paper Title]

**One-sentence summary**: [Core contribution in one sentence]
**Key points**:
- [Point 1]
- [Point 2]
- [Point 3]
**Relevance**: high / medium / low
**Next action**: read fully / extract evidence / skip
```

## Concept Note Template

Each concept note captures a single idea synthesized across multiple sources. Define the concept precisely, support it with evidence, and link to related concepts.

```markdown
# [Concept Name]

## Definition
[Precise definition with attribution to originating source]

## Key Components
- Component 1: [Description]
- Component 2: [Description]

## Evidence
- [Source 1]: [Finding supporting this concept]
- [Source 2]: [Finding extending or qualifying it]
- [Source 3]: [Finding from a different perspective]

## Implications
[What this concept means for the broader research area]

## Related Concepts
- [Link to related concept note 1]
- [Link to related concept note 2]

## Sources
[1] Author, A. et al. (Year). Title. DOI or URL
[2] Author, B. et al. (Year). Title. DOI or URL
```

## Meeting Note Template

Meeting notes attribute statements to specific participants and separate outcomes, decisions, and action items.

```markdown
# [Meeting Context]

**Date**: YYYY-MM-DD
**Participants**: [Names and roles]
**Purpose**: [One sentence]

## Key Outcomes
- [Outcome 1] — per [Name] ([context])
- [Outcome 2] — per [Name] ([context])

## Decisions
| Decision | Rationale | Owner |
|----------|-----------|-------|
| [Decision 1] | [Why] | [Who] |

## Action Items
- [ ] [Action] — [Owner] — [Deadline]
- [ ] [Action] — [Owner] — [Deadline]

## Open Questions
- [Question requiring follow-up]
- [Question requiring clarification]

## Sources
[First-hand: Participant name, meeting date]
```

## Constraints

All templates share three structural requirements:

- **Sources section**: Every note ends with numbered citations containing sufficient metadata for verification (author, year, title, stable identifier). First-hand sources use explicit attribution: "per [Name] ([context], [date])."
- **Relative cross-references**: Link to related notes using relative paths. Relative paths survive directory reorganization better than absolute paths.
- **Atomic scope**: Each note addresses one source, one concept, or one meeting. Split notes that accumulate multiple topics.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::writing_procedure/reference/academic_creation.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::writing_procedure/reference/academic_organization.md" />
`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/evidence_extraction_academic.md`
