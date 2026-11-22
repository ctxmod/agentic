# Broad Scan Procedure

## Prerequisites

- Target directory or directories identified
- File pattern parameters defined (e.g., `**/*.py`, `**/*`)
- Categorization scheme established based on discovery goal

## Steps

1. **Execute Glob pattern** against target directory using broad pattern (`{file_pattern}`). Use recursive patterns (`**/`) to capture nested structures.

2. **Examine directory structure** from returned file paths. Note layering clues: `src/`, `lib/`, `internal/`, `api/`, `cmd/`, or domain-based groupings.

3. **Categorize files** by scheme relevant to discovery objective:
   - **By type**: source code, configuration, documentation, tests
   - **By module/domain**: product, billing, auth, shared
   - **By layer**: models, services, controllers, infrastructure

4. **Count files per category** to establish scope. Record counts as metrics: "Found 142 Python files across 8 modules" or "Identified 23 configuration files."

5. **Report inventory** with category breakdown and file counts. Include directory structure observations when relevant to next steps.

## Decision Points

**Pattern too broad** (returns >1000 files): Narrow by extension or directory. Replace `**/*` with `**/*.py` or `src/**/*.go`.

**Pattern too narrow** (returns <10 files): Expand pattern or target directory. Add extensions: `**/*.{py,js,ts}`.

**Unclear categorization**: Use file extensions as fallback grouping (`.md` → documentation, `.json` → configuration, code extensions → source).

**Nested target directories**: Run Glob separately per directory to maintain attribution of files to their origin.

## Success Criteria

- Glob returns file list without errors
- All files categorized into at least one group
- Category counts recorded
- Inventory produced as structured output or report

## Common Issues

**Symbolic links causing duplicates**: Filter results to unique paths before categorization.

**Hidden directories included**: Exclude patterns like `**/node_modules/**`, `**/vendor/**`, `**/.git/**` in Glob pattern.

**Mixed-language projects**: Categorize by extension family first (compiled vs interpreted), then subdivide by language if objective requires.

**Large file counts overwhelming sampling**: Use counts to establish statistically significant sample size for subsequent detailed analysis phases.
