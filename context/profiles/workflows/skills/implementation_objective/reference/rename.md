# Rename Implementation Objective

Change identifiers with comprehensive reference updates across all consumers. Rename objectives modify names, paths, or identifiers while ensuring all references update consistently, maintaining system integrity.

## Base Behavior

the `implementation_objective` skill

## Scope

Rename implementation applies to any identifier change:

- Path changes (moving or renaming files, directories)
- Identifier changes (variable names, keys, slugs)
- Label changes (titles, display names, aliases)
- Reference updates (links, imports, cross-references)
- Alias additions (adding alternative identifiers)
- Namespace changes (prefixes, scopes, qualifiers)

Renaming changes how things are identified without changing their meaning or behavior.

## Core Behavior

1. **Identify**: Determine current identifier and target identifier
2. **Discover references**: Find all occurrences of current identifier
3. **Classify references**: Distinguish direct references from derived usages
4. **Plan updates**: Map each reference to its updated form
5. **Apply rename**: Change the primary identifier
6. **Update references**: Modify all references to use new identifier
7. **Verify**: Confirm all references resolve correctly
8. **Report**: Document the rename and reference updates

## Relationship to Other Implementation Types

| Type | Scope | Content Change |
|------|-------|----------------|
| Edit | Modify content | Changes meaning |
| Refactor | Reorganize structure | Changes organization |
| Create | Generate new | Adds content |
| Delete | Remove existing | Removes content |
| **Rename** | **Change identifier** | **Preserves meaning and structure** |

Rename objectives differ from refactor: refactors change organization; renames change identifiers while preserving organization.

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`

SSOT ensures the identifier change propagates to all references. DRY prevents identifier proliferation. Reference over duplication means references update rather than duplicating under both names.

## Pre-Rename

Before changing:

1. Confirm the new identifier does not conflict with existing identifiers
2. Understand why the rename is necessary (clarity, convention, conflict)
3. Discover all references to the current identifier
4. Classify references by type and update mechanism
5. Verify rename does not break external dependencies
6. Confirm parameters: SOURCE_IDENTIFIER, TARGET_IDENTIFIER, SCOPE

## Reference Discovery

`ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/consumers.md`

Before renaming, identify all references:

| Reference Type | Discovery Method | Update Approach |
|----------------|------------------|-----------------|
| **Path references** | Grep for exact path | Replace path strings |
| **Import statements** | Language-specific patterns | Update import syntax |
| **Identifier usage** | Grep for identifier | Replace identifier strings |
| **Documentation links** | Grep for link patterns | Update link targets |
| **Frontmatter fields** | Grep for field values | Update field values |
| **Configuration** | Grep for config values | Update config entries |

## During Rename

Apply changes with these guidelines:

- **Atomic updates**: Rename primary and update references together
- **Comprehensive scope**: Update all reference types consistently
- **Order matters**: For paths, rename source first; for identifiers, update references first
- **No partial states**: Avoid states where some references are updated and others are not
- **Preserve semantics**: The referent's meaning must not change

## Verification

After renaming:

1. Confirm the new identifier is established
2. Verify no references to old identifier remain (except historical)
3. Check all references to new identifier resolve correctly
4. Validate no naming conflicts introduced
5. Ensure behavior is unchanged

`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/references.md`
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/consistency.md`

## Tool Constraints

**Allowed**:
- Read: Examine artifacts and references
- Grep: Search for identifier occurrences
- Glob: Identify files within scope
- Edit: Update references
- Bash: File/directory rename commands (mv, git mv)
- Write: Only when Edit tool limitations require

**Forbidden**:
- Renaming without reference discovery
- Leaving stale references
- Creating identifier conflicts
- Launching sub-agents
- Renaming outside specified scope

## Response Format

After completing the rename, return a brief response:

```
Renamed {SOURCE_IDENTIFIER} → {TARGET_IDENTIFIER}.

Summary: [1-2 sentence description of the rename and rationale]

References updated: [count by type]
- Path references: N
- Import statements: N
- Documentation links: N
```

## Variants

| Variant | Focus |
|---------|-------|
| `path` | File and directory path changes |
| `identifier` | Named references and symbols |
| `alias` | Adding or changing alternative identifiers |
| `namespace` | Prefix, scope, or qualifier changes |

Variants specialize renaming to specific identifier categories.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Renaming without reference search | Broken references | Discover all occurrences first |
| Partial updates | Inconsistent state | Update all references atomically |
| Conflicting identifiers | Ambiguity | Verify uniqueness before rename |
| Semantic changes during rename | Conflated concerns | Rename preserves meaning |
| Keeping both identifiers | Confusion | Fully migrate to new identifier |
| Ignoring external references | External breakage | Document or coordinate external updates |
