# Repository Source Discovery Objective

Explore repository source code to map structure, detect entry points, and identify module boundaries. This variant produces a navigational understanding of the codebase that enables targeted exploration in subsequent tasks.

## Base Behavior

the `discovery_objective` skill

## Focus Areas

### Structure Analysis

Map the repository layout: top-level directories, source vs configuration vs documentation separation, build artifacts, and test organization. Identify the primary language(s) and framework conventions that shape directory structure.

### Entry Point Detection

Locate public interfaces that external consumers or runtime environments invoke: exported symbols, main functions, CLI commands, API routes, event handlers. Entry points reveal the codebase's intended usage surface.

### Module Boundary Recognition

Distinguish internal implementation from public API. Identify encapsulation mechanisms: package boundaries, visibility modifiers, barrel exports, internal directories. Module boundaries guide where to look for functionality and where modifications propagate.

## Methodology

### Phase 1: Structure Analysis

Brief template for a procedure-depth agent (launched from objective depth):

```text
Perform a broad scan of the repository to map its overall structure and directory layout.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`
```

**Repository source application**:
1. Parse build/package configuration files to identify language, workspace structure, and module organization
2. Build source-specific directory map distinguishing code, tests, configuration, and build artifacts
3. Note monorepo workspace definitions and shared package dependencies

### Phase 2: Entry Point Detection

Brief template for a procedure-depth agent (launched from objective depth):

```text
Detect the codebase's entry points using the language-specific patterns in the procedure.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/entry_point_detection.md`
```

**Repository source application**:
- Use Grep with language-specific patterns from the procedure to locate entry points
- Validate candidate entry points by reading files and confirming export/invocation status
- Document entry point type (API, executable, event handler) with file:line references

### Phase 3: Module Boundary Analysis

Identify encapsulation mechanisms:

- **Barrel exports**: Files that re-export from subdirectories (`index.ts`, `mod.rs`, `__init__.py`)
- **Visibility modifiers**: Language constructs limiting access (`internal`, `private`, `pub(crate)`)
- **Internal packages**: Directories named `internal`, `_private`, or prefixed with `_`
- **Package boundaries**: Separate `package.json`, `go.mod`, or similar per module

Map which modules expose public APIs and which serve as internal implementation.

## Tool Permissions

**Allowed**: Glob (file inventory), Grep (pattern search), Read (file examination), Bash (read-only: `ls`, `git log`, `tree`), Write (reserved report path only)

**Forbidden**: Modifying source files, launching sub-agents, writing outside the reserved report path, executing build commands

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Organize and write the repository source discovery report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Repository source reports include these objective-specific sections:

- **Structure**: Languages (with file counts), directory layout (with purpose annotations), build configuration
- **Entry Points**: Public APIs, executables, event handlers (all with `file:line` references)
- **Module Boundaries**: Public modules, internal modules, dependency graph

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Listing every file | Low signal, high noise | Summarize by category with counts |
| Ignoring configuration files | Miss build structure | Parse package.json, Cargo.toml, etc. |
| Language-agnostic only | Miss idiomatic patterns | Apply language-specific detection |
| Conflating tests with source | Muddies entry point analysis | Separate test directories explicitly |
| Skipping visibility analysis | Unclear module boundaries | Trace export chains to distinguish public/internal |

## Examples

**TypeScript Monorepo**: Glob `packages/*/src/**/*.ts`, identify 8 packages. Detect entry points via `index.ts` barrels (12 total). Map public exports from `packages/*/src/index.ts`. Report: 3 library packages (public), 5 internal utilities.

**Go Service**: Glob `**/*.go`, identify `cmd/` and `internal/` directories. Locate `main.go` in `cmd/server/`. Trace exported identifiers from `pkg/` (capitalized functions). Report: 1 executable entry point, 4 public packages, 6 internal packages.

**Python Library**: Glob `**/*.py`, find `src/` layout with `pyproject.toml`. Parse `__all__` from `src/library/__init__.py`. Identify 15 public symbols, 8 internal modules (prefixed `_`). Report: single package entry point, clear public/private split.

## Related

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
