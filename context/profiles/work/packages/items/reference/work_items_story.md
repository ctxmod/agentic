# Story Writing Objective

Create a story that defines a specific deliverable outcome. Stories bridge strategic epics and actionable issues. Each story articulates value, establishes acceptance criteria, and identifies implementation issues. Produce the story document at the designated STORY_PATH.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_items.md" />

## Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_structure_story.md" />

The schema defines record fields, required sections, optional sections, status lifecycle, path conventions, and validation checklist. This objective defines the behavioral process for creating stories.

## Scope

Stories represent deliverable units of value:

- Features that provide observable capability
- Enhancements that measurably improve existing functionality
- Documentation that serves identified audiences
- Research deliverables with defined outputs
- Process changes with observable effects
- Configuration or setup with verifiable results

Stories decompose into issues for implementation tracking.

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`

SSOT ensures stories are the authoritative source for feature scope—issues reference stories rather than duplicating acceptance criteria. Context efficiency keeps story content focused on deliverable value. Reference over duplication encourages linking to parent epics rather than restating strategic context.

## Core Behavior

1. **Understand**: Clarify the value this story delivers and to whom
2. **Bound**: Define acceptance criteria that verify completion
3. **Decompose**: Identify issues required to deliver the story
4. **Contextualize**: Link to parent epic and related stories
5. **Write**: Produce story document at STORY_PATH

## Value Articulation

Every story must articulate value in terms of:

| Component | Question | Example Pattern |
|-----------|----------|-----------------|
| Recipient | Who benefits? | "Users", "Operators", "Downstream consumers" |
| Capability | What can they do? | "can verify", "will receive", "no longer need to" |
| Benefit | Why does it matter? | "reducing errors", "enabling automation" |

Avoid stories that describe activities ("implement X") rather than outcomes ("X works correctly").

## Pre-Writing

Before drafting:

1. Review parent epic for strategic context
2. Identify related stories to understand boundaries
3. Gather requirements from stakeholders or source materials
4. Confirm STORY_PATH parameter is available

`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`

## During Writing

`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`

While authoring:

- Verify each acceptance criterion is testable
- Ensure issues cover all acceptance criteria
- Check for unstated assumptions that affect scope
- Use concrete terminology matching the domain
- Reference source materials for traced requirements

## Document Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the story document following the report creation and organization procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Story documents follow report creation patterns. Stories reference their parent epic. Story completion requires all issues resolved and acceptance criteria verified.

## Tool Constraints

**Allowed**:
- Read: Epic, related stories, requirements documents, schema
- Glob: Find related artifacts
- Grep: Search for dependencies
- Write: Story document at STORY_PATH only

**Forbidden**:
- Writing issues (separate objective)
- Modifying existing artifacts
- Writing outside STORY_PATH

## Response Format

After writing the story, return a brief response:

```
Wrote story to {STORY_PATH}.

Summary: [2-3 sentence summary of deliverable value and issue count]
```

Do not return the full story content. The document is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Activity titles ("Implement X") | No clear outcome | Outcome titles ("X functions correctly") |
| Vague acceptance criteria | Untestable completion | Specific, binary conditions |
| Missing value statement | Unclear purpose | Articulate recipient and benefit |
| Orphan stories | Lost context | Link to parent epic |
| Scope creep | Unmanageable size | Use "Out of Scope" section |
| Implementation details | Premature decisions | Describe outcomes, not methods |
