# Work Item Writing Objective

Create work items that define actionable work with clear scope and acceptance criteria. Work items form a hierarchy from strategic initiatives to granular activities. Each level provides appropriate context for its scope while referencing parent items for broader context.

## Base Behavior

the `writing_objective` skill

## Work Item Schemas

Work item structure is defined in schema files. Reference the appropriate schema for structural requirements:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_structure_epic.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_structure_story.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_structure_issue.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_structure_task.md" />

Schemas define record fields, required sections, optional sections, and validation checklists. This objective defines the behavioral process for creating work items.

## Work Item Hierarchy

Work items organize in a four-level hierarchy:

| Level | Scope | Contains | Deliverables |
|-------|-------|----------|--------------|
| Epic | Initiative | Stories | Strategic outcomes |
| Story | Feature | Issues | Deliverable value |
| Issue | Work unit | Tasks (optional) | Completed work |
| Task | Activity | None | Granular progress |

Each level inherits context from its parent and provides context to its children.

## Variant Objectives

Specialized objectives exist for each work item type:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_items_epic.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_items_story.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_items_issue.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_items_task.md" />

## Core Behavior

1. **Identify**: Determine the appropriate work item type for the scope
2. **Reference**: Read the corresponding schema for structural requirements
3. **Gather**: Collect context from parent items and source materials. Preferred: `ctxmod work show <parent_id>`. Fallback: read the parent item's `ITEM.md` directly.
4. **Scope**: Define boundaries—what belongs and what does not
5. **Criteria**: Establish verifiable completion conditions
6. **Write**: Produce the work item body at its `ITEM.md` (the path `ctxmod work new` printed, or the item directory passed in)
7. **Summarize**: Return brief response with path

## Parent-Child Relationships

Work items reference their parents for context:

- **Epics**: Standalone at initiative level
- **Stories**: Reference parent epic for strategic context
- **Issues**: Reference parent story for feature context
- **Tasks**: Reference parent issue for work context

Child items do not duplicate parent content—they link to it.

## Work Item Creation

Work items are created at designated paths:

| Type | Path Parameter |
|------|----------------|
| Epic | EPIC_PATH |
| Story | STORY_PATH |
| Issue | ISSUE_PATH |
| Task | TASK_PATH |

`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`

## Tool Constraints

**Allowed**:
- Read: Parent items, related work items, source materials, schemas
- Glob: Find related artifacts
- Grep: Search for dependencies
- Write: Work item at designated path only

**Forbidden**:
- Writing child items (separate objectives)
- Modifying existing artifacts
- Writing outside designated path

## Response Format

After writing the work item, return a brief response:

```
Wrote {work item type} to {PATH}.

Summary: [2-3 sentence summary of scope and contained items]
```

Do not return full work item content. The document is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Vague success criteria | Untestable completion | Specific, binary conditions |
| Activity-focused titles | No clear outcome | Outcome-oriented phrasing |
| Missing scope definition | Ambiguous boundaries | Explicit include/exclude |
| Duplicating parent context | Redundancy | Reference parent items |
| Orphan items | Lost context | Link to parent hierarchy |
| Single massive item | Unmanageable scope | Decompose appropriately |
