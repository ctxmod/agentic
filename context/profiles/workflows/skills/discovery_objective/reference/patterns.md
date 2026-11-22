# Pattern Discovery Objective

Extract and catalog recurring patterns from files to identify reusable structures, detect inconsistencies, and find consolidation opportunities. Pattern discovery analyzes existing implementations to establish conventions that maintain consistency across a codebase.

## Base Behavior

the `discovery_objective` skill

## Pattern Categories

### Structural Patterns

Directory organization, file groupings, section hierarchy. Look for: nested hierarchies (`{category}/{subcategory}/{topic}`), flat organization by type, domain-based grouping, or architectural layering. Identify organizational patterns and document the convention.

### Metadata Patterns

YAML field conventions, required vs optional fields, field ordering. Look for: standardized field sets, validation schemas, metadata conventions, or version identifiers. Note which fields appear universally vs situationally.

### Content Patterns

Word count ranges by category, density guidelines, section length conventions. Analyze: section lengths, paragraph structures, information density, prose vs code ratios. Establish whether documentation follows concise, verbose, or mixed styles.

### Cross-Reference Patterns

Cross-reference formats (tags, imports, or include directives), link styles (required vs optional), name resolution patterns for references. Identify: structured reference systems, inline embedding mechanisms, external linking conventions.

### Naming Patterns

File naming conventions, identifier formats, slug patterns, metadata name fields. Look for: casing conventions (kebab-case, camelCase, snake_case), hierarchical naming, category prefixes, or semantic naming schemes.

### Anti-Patterns

Inconsistent pattern application, one-off implementations that should follow conventions, violations of established structures.

## Discovery Methodology

### Phase 1: Broad Scan

Brief template for a procedure-depth agent (launched from objective depth):

```text
Perform a broad scan to establish the file set and category counts for pattern analysis.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`
```

**Objective-specific application**: Use pattern category counts to calculate threshold requirements for subsequent validation phases.

### Phase 2: Pattern Detection

Brief template for a procedure-depth agent (launched from objective depth):

```text
Detect recurring patterns across the six pattern categories using frequency, similarity, and structural strategies.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/pattern_detection.md`
```

**Objective-specific application**: Apply frequency, similarity, and structural detection strategies to identify the six pattern categories: structural, metadata, content, cross-reference, naming, and anti-patterns.

### Phase 3: Pattern Validation

Verify minimum thresholds: patterns must appear in 3+ files or 50%+ of category files (whichever is greater). Distinguish intentional conventions from coincidental similarity. Identify valid exceptions with documented reasons for pattern deviation.

### Phase 4: Pattern Cataloging

Document pattern name, type, and frequency. Provide concrete examples with `file:line` references. Note related patterns and anti-patterns. Identify consolidation opportunities (DRY violations where patterns could eliminate duplication).

## Tool Permissions

**Allowed**: Glob (file sets), Grep (pattern search), Read (file examination), Write (reserved report path only)

**Forbidden**: Modifying source files, launching sub-agents, writing outside the reserved report path

## Report Structure

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the pattern discovery report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Pattern discovery reports include these objective-specific sections:

| Section | Content |
|---------|---------|
| Patterns by Category | Subsections for structural, metadata, content, cross-reference, and naming patterns |
| Anti-Patterns | Violations with specific file:line evidence |
| Consolidation Opportunities | DRY violations where pattern application could reduce duplication |
| Related Files | Files examined during discovery |

Each pattern entry includes: pattern name, frequency (e.g., "12/15 files"), and examples with file:line references.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Extracting one-off occurrences | Coincidences, not conventions | Require 3+ instances minimum |
| Missing line references | Unverifiable claims | Include `file:line` for all examples |
| Ignoring scope limits | Noise from unrelated files | Restrict to target category |
| Pattern without purpose | Documentation without guidance | Link patterns to planning application |
| Conflicting patterns without context | Confuses application | Note when and why patterns differ |

## Examples

**API Route Pattern Discovery**: Glob `api/routes/**/*.js`, examine 30 files. Identify structural pattern: all use `router.{method}('{path}', handler)` structure. Find authentication pattern: 25/30 files use middleware pattern. Report 6 structural, 3 authentication, 2 validation patterns.

**Configuration Pattern Discovery**: Glob `config/*.yaml`, examine 15 files. Detect section pattern: all use 3-level hierarchy (service/environment/setting). Find validation pattern: schema references in 12/15 files. Identify anti-pattern: 3 files missing environment inheritance.

**Documentation Pattern Discovery**: Glob `docs/**/*.md`, examine 50 files. Extract heading pattern: all use H2 sections (200-500 words). Find metadata pattern: 100% have `title` and `description` fields. Catalog cross-reference pattern: relative links with `../` navigation.

## Related

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
