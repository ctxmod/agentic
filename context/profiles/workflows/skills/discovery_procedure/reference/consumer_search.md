# Consumer Search Procedure

Systematically identify files that reference, import, or depend on a target to assess change impact and understand coupling patterns.

## Prerequisites

- Target file path or concept identified
- Search scope defined (directories to examine)
- Target file read to extract identifiers

## Steps

### Phase 1: Extract Searchable Identifiers

Analyze the target file to compile identifiers for searching:

1. **File path and name patterns**: Base name, directory structure, full path
2. **Exported symbols**: Functions, classes, types, constants
3. **Frontmatter fields**: `name`, `key`, `aliases` values
4. **Unique strings**: Distinct identifiers, terminology, or conventions

### Phase 2: Execute Multi-Pattern Search

Search across target directories using identifier-specific patterns:

**Grep for exact references**:
- Path literals: full or relative file paths
- Name field values: `name:` or `key:` frontmatter matches
- Import patterns: language-specific import statements
- Link references: markdown links, cross-references

**Glob for likely consumers**:
- Directories that typically reference the target category
- File types that commonly import or link to targets
- Related naming patterns (prefixes, suffixes)

**Read candidate files** to verify reference context and confirm actual dependency relationships.

### Phase 3: Expand Search Scope

For each match, consider related references:
- A match for `discovery.md` suggests checking `discovery-source.md`, `discovery-consumer.md`
- Import references to `utils/helpers.ts` suggests checking sibling files
- Name pattern matches (e.g., `agent:discovery-*`) indicate variant consumers

### Phase 4: Classify by Dependency Strength

Categorize each confirmed consumer:

| Strength | Characteristics | Search Patterns |
|----------|----------------|-----------------|
| **Hard** | Imports, requires, inline tags | `import.*target`, `require(.*target)`, inline tag patterns |
| **Soft** | Links, references, mentions | `[text](target)`, `Read: target`, link tag patterns |
| **Implicit** | Follows patterns from target | Same naming scheme, structural parallels |

## Decision Points

**No matches found**: Expand search scope. Check parent directories, try partial matches, search for loose terminology or concepts from the target.

**Too many matches (>100)**: Narrow by identifier specificity. Use exact symbol names instead of general patterns, focus on hard dependencies first.

**Ambiguous reference context**: Read surrounding 20 lines of matched files to determine whether the match represents actual dependency or coincidental string occurrence.

**Cascading dependencies detected**: Flag consumers that themselves have consumers. These represent high-impact propagation points requiring priority attention.

## Success Criteria

- All identifier types searched across target scope
- Consumers categorized by dependency strength
- Cascading dependencies identified and flagged
- Search patterns documented for reproducibility
- Results structured for impact assessment

## Common Issues

**False positives from common names**: Use context verification (Read tool) to confirm actual references. Distinguish `import { config }` from unrelated `config` variable names.

**Missing indirect consumers**: Semantic dependencies lack explicit references. Look for files following target's patterns, conventions, or architectural choices without direct imports.

**Search pattern over-matching**: Escape special regex characters in identifiers. Use word boundaries (`\b`) when searching for symbol names.

**Language-specific import syntax**: Search appropriate patterns for each language type (Python imports, JavaScript requires, Go imports, Rust use statements).

## Search Pattern Templates

### Import Patterns

```regex
import.*from.*['"].*TARGET['"]
import.*TARGET
require\(['"].*TARGET['"]\)
@import.*TARGET
use.*TARGET::.*
from.*TARGET.*import
```

### Frontmatter References

```regex
related:.*TARGET
path:.*TARGET
name:\s*TARGET
key:\s*TARGET
```

### Markdown Links

```regex
\[.*\]\(.*TARGET.*\)
Read:.*TARGET
\[\[.*TARGET.*\]\]
```

### Inline Tags

Template-tag patterns (Liquid/Jinja-style include and link tags in context or documentation sources) follow the format: `OPEN_BRACE PERCENT ... PERCENT CLOSE_BRACE`

```regex
inline tag pattern: \{PERCENT.*inline.*TARGET.*PERCENT\}
link tag pattern: \{PERCENT.*link.*TARGET.*PERCENT\}
agent:\s*TARGET
skill:\s*TARGET
```

Replace `PERCENT` with `%` in actual searches.
