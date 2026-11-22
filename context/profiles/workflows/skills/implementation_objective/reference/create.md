# Create Implementation Objective

Generate new artifacts that fulfill specified requirements. Create objectives produce complete, well-formed artifacts that integrate with existing structures and follow established conventions.

## Base Behavior

the `implementation_objective` skill

## Scope

Create implementation applies to any new artifact generation:

- New files (content, configuration, data, specifications)
- New sections (adding components to existing artifacts)
- New entries (records, items, elements in collections)
- New structures (directories, hierarchies, organizational units)
- New definitions (types, schemas, interfaces, contracts)
- New relationships (links, references, associations)

Creation adds artifacts that did not previously exist.

## Core Behavior

1. **Understand**: Clarify what the new artifact must accomplish
2. **Pattern**: Identify existing artifacts to use as models
3. **Design**: Define structure and content for the new artifact
4. **Generate**: Create the artifact following patterns and requirements
5. **Integrate**: Establish connections with existing artifacts
6. **Verify**: Confirm the artifact meets requirements and conventions
7. **Report**: Document what was created and its purpose

## Relationship to Other Implementation Types

| Type | Scope | Artifact State |
|------|-------|----------------|
| Edit | Modify existing | Artifact exists |
| Refactor | Reorganize existing | Artifact exists |
| **Create** | **Generate new** | **Artifact does not exist** |
| Delete | Remove existing | Artifact exists |
| Rename | Relabel existing | Artifact exists |

Create objectives differ from edit: edits modify existing content; creates generate new artifacts from requirements.

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/convention_over_configuration.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`

KISS ensures new artifacts remain focused on their purpose. Convention over configuration guides using established patterns. DRY prevents creating content that duplicates existing artifacts.

## Pre-Create

Before generating:

1. Confirm the artifact does not already exist
2. Identify similar artifacts to use as patterns
3. Understand conventions governing this artifact type
4. Gather requirements and acceptance criteria
5. Determine integration points with existing artifacts
6. Confirm TARGET_PATH parameter specifies the destination

## Pattern Discovery

New artifacts should follow established patterns:

1. **Locate exemplars**: Find existing artifacts of the same type
2. **Extract patterns**: Identify structural conventions, required elements, style
3. **Document deviations**: Note where requirements differ from patterns
4. **Justify variance**: Ensure deviations have explicit rationale

`ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/patterns.md`

## During Create

Apply generation with these guidelines:

- **Follow patterns**: Match structure and style of similar artifacts
- **Meet requirements**: Include all specified content and capabilities
- **Establish integration**: Create necessary references and links
- **Maintain consistency**: Align with existing conventions
- **Document purpose**: Include metadata or context explaining the artifact

## Verification

After creating:

1. Confirm the artifact is well-formed and complete
2. Verify it follows conventions for its type
3. Check that integration points connect correctly
4. Validate against acceptance criteria
5. Ensure no unintended duplication with existing artifacts

`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/structural.md`
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/completeness.md`

## Tool Constraints

**Allowed**:
- Read: Examine patterns and related artifacts
- Grep: Search for conventions and integration points
- Glob: Identify similar artifacts for patterns
- Write: Create new artifact at TARGET_PATH
- Edit: Modify existing artifacts to integrate new artifact

**Forbidden**:
- Creating outside specified TARGET_PATH
- Duplicating existing artifact content
- Launching sub-agents
- Skipping pattern discovery

## Response Format

After completing creation, return a brief response:

```
Created {TARGET_PATH}.

Summary: [1-2 sentence description of what was created and its purpose]

Pattern source: [path to exemplar used, if applicable]
```

## Variants

| Variant | Focus |
|---------|-------|
| `content` | Text, documentation, narrative artifacts |
| `structure` | Directories, hierarchies, organizational artifacts |
| `definition` | Types, schemas, interfaces, contracts |
| `configuration` | Settings, parameters, environment artifacts |

Variants specialize creation to specific artifact categories.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Creating without pattern search | Inconsistent artifacts | Find and follow exemplars |
| Duplicating existing content | DRY violation | Reference or extend instead |
| Skipping integration | Orphaned artifacts | Establish connections |
| Over-specification | Premature complexity | Create minimal viable artifact |
| Ignoring conventions | Inconsistent ecosystem | Match existing patterns |
| Creating without verification | Defective artifacts | Validate against requirements |
