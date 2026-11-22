# Delete Implementation Objective

Remove artifacts safely with proper dependency handling. Delete objectives eliminate content that is obsolete, duplicated, or no longer needed while ensuring consumers are updated or alternatives provided.

## Base Behavior

the `implementation_objective` skill

## Scope

Delete implementation applies to any artifact removal:

- File deletion (removing entire artifacts)
- Section removal (eliminating components from existing artifacts)
- Entry removal (deleting records, items, elements from collections)
- Structure removal (removing directories, hierarchies)
- Definition removal (eliminating types, schemas, interfaces)
- Reference removal (breaking associations, links)

Deletion subtracts artifacts from the ecosystem.

## Core Behavior

1. **Identify**: Confirm what should be removed and why
2. **Discover consumers**: Find all artifacts that reference the target
3. **Assess impact**: Evaluate consequences of removal for each consumer
4. **Plan migration**: Determine how consumers will adapt
5. **Update consumers**: Modify references before deletion
6. **Remove**: Delete the target artifact
7. **Verify**: Confirm no broken references remain
8. **Report**: Document what was removed and consumer updates

## Relationship to Other Implementation Types

| Type | Scope | Result |
|------|-------|--------|
| Edit | Modify content | Artifact changed |
| Refactor | Reorganize structure | Artifact reorganized |
| Create | Generate new | Artifact added |
| **Delete** | **Remove existing** | **Artifact removed** |
| Rename | Relabel | Artifact renamed |

Delete objectives differ from edit: edits change content; deletes remove entire artifacts or components.

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

SSOT may motivate deletion when content is duplicated elsewhere. Reference over duplication guides updating consumers to point to authoritative sources. Context efficiency justifies removing obsolete content.

## Pre-Delete

Before removing:

1. Confirm the artifact is genuinely obsolete or unnecessary
2. Identify why deletion is appropriate (obsolete, duplicate, superseded)
3. Discover all consumers of the target artifact
4. Assess impact on each consumer
5. Plan consumer migration or update strategy
6. Confirm TARGET_PATH parameter identifies what to delete

## Consumer Discovery

`ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/consumers.md`

Before deletion, identify all consumers:

1. **Search for references**: Direct imports, links, paths
2. **Search for identifiers**: Named references, aliases, keys
3. **Search for semantic dependencies**: Patterns or conventions based on target
4. **Classify by impact**: Hard dependencies vs soft references
5. **Document consumer set**: List all affected artifacts

## Impact Assessment

For each consumer, determine:

| Impact Level | Description | Required Action |
|--------------|-------------|-----------------|
| **Breaking** | Consumer fails without target | Must update before delete |
| **Degrading** | Consumer works with reduced quality | Should update before delete |
| **Optional** | Consumer unaffected or minimally affected | May update after delete |

## During Delete

Apply removal with these guidelines:

- **Update consumers first**: Never leave broken references
- **Provide alternatives**: Point consumers to replacement if one exists
- **Remove cleanly**: Delete all parts of the artifact (including metadata)
- **Preserve history**: Use version control; avoid destroying audit trail
- **Document rationale**: Explain why deletion occurred

## Verification

After deleting:

1. Confirm target artifact no longer exists
2. Verify no broken references remain in consumers
3. Check that replacements (if any) are accessible
4. Validate consumers function correctly post-deletion
5. Ensure no orphaned related artifacts exist

`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/references.md`

## Tool Constraints

**Allowed**:
- Read: Examine target and consumers
- Grep: Search for references
- Glob: Identify affected files
- Edit: Update consumers before deletion
- Bash: File deletion commands (rm, git rm)
- Write: Only for consumer updates

**Forbidden**:
- Deleting without consumer discovery
- Leaving broken references
- Launching sub-agents
- Deleting outside specified scope

## Response Format

After completing deletion, return a brief response:

```
Deleted {TARGET_PATH}.

Summary: [1-2 sentence description of what was removed and why]

Consumers updated: [count or list of updated consumers]
Replacement: [path to alternative, if applicable]
```

## Variants

| Variant | Focus |
|---------|-------|
| `artifact` | Complete file or resource removal |
| `component` | Section or element removal from existing artifact |
| `obsolete` | Removing outdated content with replacement available |
| `duplicate` | Removing redundant content (SSOT cleanup) |

Variants specialize deletion to specific removal scenarios.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Deleting without consumer search | Broken references | Discover consumers first |
| Leaving orphaned references | Degraded quality | Update all consumers |
| Deleting without rationale | Unclear history | Document why deletion occurred |
| Deleting and recreating | Churn | Use edit or refactor instead |
| Assuming no consumers | Hidden dependencies | Search comprehensively |
| Partial deletion | Inconsistent state | Remove all related content |
