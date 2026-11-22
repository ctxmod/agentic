# Epic Writing Objective

Create an epic that defines a major initiative spanning multiple stories. Epics establish scope boundaries, articulate strategic value, and provide measurable success criteria. Produce the epic document at the designated EPIC_PATH.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_items.md" />

## Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_structure_epic.md" />

The schema defines record fields, required sections, optional sections, status lifecycle, path conventions, and validation checklist. This objective defines the behavioral process for creating epics.

## Scope

Epics represent large initiatives decomposable into stories:

- Product capabilities requiring multiple coordinated changes
- Organizational initiatives spanning teams or domains
- Research programs with phased deliverables
- Infrastructure improvements affecting multiple systems
- Process transformations with sequential milestones
- Documentation efforts covering broad subject areas

Epics provide strategic context that stories inherit.

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`

SSOT ensures epics are the authoritative source for initiative scope—stories reference epics rather than duplicating context. Context efficiency keeps epic content focused on strategic value. DRY prevents scattering initiative details across multiple documents.

## Core Behavior

1. **Understand**: Clarify the initiative's strategic purpose and boundaries
2. **Scope**: Define what belongs in this epic and what does not
3. **Decompose**: Identify constituent stories at appropriate granularity
4. **Measure**: Establish concrete success criteria
5. **Write**: Produce epic document at EPIC_PATH

## Pre-Writing

Before drafting:

1. Gather strategic context from stakeholders or source materials
2. Identify related initiatives to avoid overlap
3. Understand constraints affecting scope or timeline
4. Confirm EPIC_PATH parameter is available

`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`

## During Writing

`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`

While authoring:

- Use concrete language—avoid vague terms like "improve" or "enhance" without specifics
- Ensure each success criterion is independently verifiable
- Check that stories collectively cover the full scope
- Validate that exclusions prevent scope creep
- Reference source materials for claims requiring attribution

## Document Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the epic document following the report creation and organization procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Epic documents follow report creation patterns. Stories reference their parent epic. Epic completion requires all stories resolved or explicitly descoped.

## Tool Constraints

**Allowed**:
- Read: Reference materials, related epics, context documents, schema
- Glob: Find related artifacts
- Grep: Search for dependencies or conflicts
- Write: Epic document at EPIC_PATH only

**Forbidden**:
- Writing stories (separate objective)
- Modifying existing artifacts
- Writing outside EPIC_PATH

## Response Format

After writing the epic, return a brief response:

```
Wrote epic to {EPIC_PATH}.

Summary: [2-3 sentence summary of initiative scope and story count]
```

Do not return the full epic content. The document is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Vague success criteria | Unverifiable completion | Specify measurable outcomes; include at least one falsifying test. |
| Stories without boundaries | Scope creep | Define story scope explicitly |
| Missing exclusions | Ambiguous boundaries | State what is out of scope |
| Activity-focused titles | Unclear outcome | Use outcome-oriented phrasing |
| Overlapping with other epics | Duplicate work | Reference related initiatives |
| Single massive epic | Unmanageable scope | Split into focused initiatives |
