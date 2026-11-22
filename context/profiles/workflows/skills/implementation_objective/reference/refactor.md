# Refactor Implementation Objective

Reorganize artifact structure while preserving meaning and behavior. Refactor objectives improve organization, reduce duplication, enhance clarity, or align with conventions without changing what the artifact accomplishes.

## Base Behavior

the `implementation_objective` skill

## Scope

Refactor implementation applies to any artifact requiring structural improvement:

- Organization changes (reordering sections, regrouping elements)
- Extraction (moving content to new locations, creating shared artifacts)
- Consolidation (merging duplicated content, unifying patterns)
- Decomposition (breaking large artifacts into focused units)
- Hierarchy adjustment (changing nesting levels, restructuring relationships)
- Convention alignment (adopting new organizational patterns)

Refactoring changes how content is organized without changing its meaning.

## Core Behavior

1. **Assess**: Understand current structure and its deficiencies
2. **Design**: Plan the target structure that addresses deficiencies
3. **Map**: Identify how current elements move to target structure
4. **Transform**: Apply structural changes systematically
5. **Verify**: Confirm meaning and behavior are preserved
6. **Update**: Adjust references affected by structural changes
7. **Report**: Document structural changes made

## Relationship to Other Implementation Types

| Type | Scope | Behavioral Impact |
|------|-------|-------------------|
| Edit | Content changes | May change behavior |
| **Refactor** | **Structure changes** | **Preserves behavior** |
| Create | New artifacts | Adds behavior |
| Delete | Artifact removal | Removes behavior |
| Rename | Identifier changes | Preserves behavior |

Refactor objectives differ from edit: edits change what content says; refactors change how content is organized.

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`

DRY motivates consolidating duplicated content. KISS guides toward simpler organization. SSOT ensures refactoring eliminates rather than creates multiple sources of truth.

## Pre-Refactor

Before reorganizing:

1. Read all artifacts within refactoring scope
2. Identify the structural problem being addressed
3. Document current organization and its deficiencies
4. Design target organization with explicit rationale
5. Identify references that will require updates
6. Confirm scope parameters (TARGET_PATH or TARGET_PATHS)

## Refactoring Strategies

Select approach based on structural problem:

| Strategy | When to Use |
|----------|-------------|
| **Extract** | Content belongs in a separate artifact |
| **Inline** | Indirection adds complexity without value |
| **Consolidate** | Duplicated content should be unified |
| **Decompose** | Large artifact should become multiple focused units |
| **Reorder** | Current sequence hinders comprehension |
| **Restructure** | Hierarchy does not match logical relationships |

## During Refactor

Apply transformations with these guidelines:

- **Preserve meaning**: Content significance must not change
- **Maintain completeness**: No content loss during transformation
- **Update references**: All pointers to moved content must update
- **Incremental changes**: Transform in verifiable steps when possible
- **Document rationale**: Explain why structure changed

## Verification

After refactoring:

1. Confirm all original content exists in target structure
2. Verify meaning equivalence between original and refactored forms
3. Check that all references resolve to correct locations
4. Validate target structure addresses the identified deficiencies
5. Ensure refactored artifacts remain well-formed

`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/consistency.md`

## Tool Constraints

**Allowed**:
- Read: Examine artifacts within scope
- Grep: Search for references requiring updates
- Glob: Identify affected files
- Edit: Modify existing artifacts
- Write: Create new artifacts when extraction requires
- Bash: Read-only commands for verification

**Forbidden**:
- Changing content meaning (use Edit objective)
- Modifying artifacts outside scope
- Launching sub-agents
- Leaving broken references

## Response Format

After completing the refactor, return a brief response:

```
Refactored {TARGET_PATH(S)}.

Summary: [2-3 sentence description of structural changes and rationale]

References updated: [count or list of updated references]
```

## Variants

| Variant | Focus |
|---------|-------|
| `extract` | Moving content to new artifacts |
| `consolidate` | Merging duplicated content |
| `decompose` | Breaking large artifacts into units |
| `reorganize` | Restructuring internal organization |

Variants specialize refactoring to specific transformation types.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Changing meaning during refactor | Conflated concerns | Separate behavioral changes |
| Leaving broken references | Incomplete refactor | Update all pointers |
| Big-bang transformations | Risky, hard to verify | Incremental steps |
| Refactoring without rationale | Arbitrary changes | Document the structural problem |
| Scope creep | Unbounded work | Define explicit boundaries |
| Improving content while reorganizing | Mixed objectives | Refactor first, then edit |
