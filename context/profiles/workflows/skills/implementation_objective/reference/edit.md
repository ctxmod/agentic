# Edit Implementation Objective

Perform targeted modifications to existing artifacts. Edit objectives make precise, bounded changes that address specific requirements without affecting unrelated content. Changes preserve surrounding context and maintain artifact integrity.

## Base Behavior

the `implementation_objective` skill

## Scope

Edit implementation applies to any artifact requiring modification:

- Content updates (text, values, identifiers, references)
- Structural adjustments (section reordering, nesting changes)
- Error corrections (typos, broken references, invalid values)
- Enhancement insertions (adding elements within existing structure)
- Constraint satisfaction (adjusting content to meet requirements)
- Format alignment (adapting to changed conventions)

Edits are bounded modifications—they change specific elements while preserving the artifact's overall structure and purpose.

## Core Behavior

1. **Locate**: Identify the precise location requiring modification
2. **Understand**: Comprehend surrounding context before changing
3. **Bound**: Define the minimal change that satisfies the requirement
4. **Modify**: Apply the change with minimal disruption
5. **Verify**: Confirm the change achieves its purpose without side effects
6. **Report**: Document what changed and why

## Relationship to Other Implementation Types

| Type | Scope | Structural Impact |
|------|-------|-------------------|
| **Edit** | **Targeted content changes** | **Minimal—preserves structure** |
| Refactor | Structure reorganization | High—changes organization |
| Create | New artifact generation | Additive—new structure |
| Delete | Artifact removal | Subtractive—removes structure |
| Rename | Identifier changes | Cross-cutting—updates references |

Edit objectives differ from refactor: edits change content while preserving structure; refactors change structure while preserving behavior.

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

KISS ensures edits remain focused—change only what the requirement demands. DRY prevents introducing duplication through edits. Context efficiency keeps modifications minimal and well-bounded.

## Pre-Edit

Before modifying:

1. Read the target artifact to understand current state
2. Identify the specific elements requiring modification
3. Understand how modified elements relate to surrounding content
4. Determine success criteria for the edit
5. Confirm TARGET_PATH parameter identifies the artifact

## During Edit

Apply modifications with these guidelines:

- **Minimal scope**: Change only what the requirement specifies
- **Context preservation**: Maintain relationships with unchanged elements
- **Style consistency**: Match existing conventions in the artifact
- **Reference integrity**: Update or preserve cross-references as appropriate
- **No feature creep**: Resist improving unrelated content

## Verification

After editing:

1. Confirm the modification achieves its stated purpose
2. Verify surrounding content remains intact
3. Check that references resolve correctly
4. Ensure style consistency with unchanged portions
5. Validate the artifact remains well-formed

`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/structural.md`

## Tool Constraints

**Allowed**:
- Read: Examine target artifact and related files
- Grep: Search for related content or references
- Glob: Identify impacted files
- Edit: Modify target artifact(s) at specified locations
- Write: Only when Edit tool limitations require full rewrite

**Forbidden**:
- Creating new artifacts (use Create objective)
- Reorganizing structure (use Refactor objective)
- Modifying unrelated content
- Launching sub-agents

## Response Format

After completing the edit, return a brief response:

```
Edited {TARGET_PATH}.

Summary: [1-2 sentence description of what changed and why]
```

Include any verification findings if relevant.

## Variants

| Variant | Focus |
|---------|-------|
| `content` | Text, values, and data modifications |
| `structure` | Element additions or removals within existing structure |
| `reference` | Cross-reference and link updates |
| `format` | Style and convention adjustments |

Variants narrow edit scope to specific modification types.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Editing without reading | Breaks context | Read artifact first |
| Scope creep | Unintended changes | Bound modifications tightly |
| Style mismatches | Inconsistent artifact | Match existing conventions |
| Orphaning references | Broken dependencies | Update or preserve links |
| Refactoring during edit | Conflated concerns | Separate edit from refactor |
| Speculative improvements | Unauthorized changes | Change only what is requested |
