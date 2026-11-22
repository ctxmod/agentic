# Dependency Discovery Objective

Analyze import statements, module relationships, and package dependencies to map structural connections within a codebase. Dependency discovery produces graphs, metrics, and anti-pattern reports that inform refactoring decisions and architecture validation.

## Base Behavior

the `discovery_objective` skill

## Dependency Categories

Dependency analysis operates across three layers:

### Code-Level Dependencies

Import statements and type references between source files. The agent parses `import`, `require`, `use`, and similar statements to identify direct dependencies. Type-only imports receive separate tracking since they affect compilation but not runtime.

### Module-Level Dependencies

Coupling relationships between logical units (packages, modules, folders). The agent computes afferent coupling (Ca: incoming dependencies) and efferent coupling (Ce: outgoing dependencies) to identify high-risk modules. Cyclical dependencies receive priority flagging.

### Package-Level Dependencies

External packages from manifests (`package.json`, `requirements.txt`, `go.mod`, `Cargo.toml`). The agent distinguishes production dependencies, development dependencies, and transitive dependencies. Version constraints and outdated packages appear in findings.

## Discovery Methodology

### Phase 1: Import Extraction

Brief template for a procedure-depth agent (launched from objective depth):

```text
Extract import statements, type references, and module declarations across source files.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/import_extraction.md`
```

**Application**: Focus dependency extraction on import statements, type references, and module declarations. Prioritize language-specific patterns that distinguish value imports from type-only imports to enable accurate runtime dependency analysis.

### Phase 2: Graph Construction

Brief template for a procedure-depth agent (launched from objective depth):

```text
Construct the dependency graph with directed edges and edge metadata from the extracted imports.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/graph_construction.md`
```

**Application**: Construct dependency graphs with directed edges flowing from dependent to dependency. Capture edge metadata including import type (value, type, dynamic), source location, and whether dependencies are internal or external to enable targeted health analysis.

### Phase 3: Analysis

Apply graph algorithms to detect dependency-specific patterns:

- **Cycle detection**: DFS traversal identifies circular dependency chains
- **Coupling metrics**: Compute Ca, Ce, and Instability (I = Ce / (Ca + Ce))
- **Critical path**: Identify modules with highest transitive dependent count
- **Orphans**: Find unreferenced exports and unused imports

### Phase 4: Cataloging

Document findings with file:line references. Group by severity: cycles (high), high coupling (medium), unused imports (low). Include concrete refactoring suggestions where applicable.

## Dependency Health Metrics

| Metric | Formula | Interpretation |
|--------|---------|----------------|
| Afferent Coupling (Ca) | Count of incoming dependencies | High Ca = widely used, stable interface expected |
| Efferent Coupling (Ce) | Count of outgoing dependencies | High Ce = fragile, affected by many changes |
| Instability (I) | Ce / (Ca + Ce) | 0 = stable, 1 = unstable |
| Cycle Count | Graph cycle enumeration | 0 = healthy, >0 = refactor candidate |

## Tool Permissions

**Allowed**: Glob (file discovery), Grep (import search), Read (file examination), Bash (read-only: `ls`, `git`, package manager queries), Write (reserved report path only)

**Forbidden**: Modifying source files, launching sub-agents, writing outside the reserved report path, running package install commands

## Report Structure

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the dependency discovery report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Dependency discovery reports include these objective-specific sections:

| Section | Content |
|---------|---------|
| Dependency Graph Summary | Node count, edge count, max depth, cycles detected |
| Dependencies by Category | Code-level (imports), module-level (coupling), package-level (external) |
| Anti-Patterns Detected | Cycles, high coupling, unused imports with file:line |
| Health Metrics | Ca/Ce/I per module, sorted by risk |
| High-Risk Dependencies | Transitive depth, outdated packages, security notes |

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Analyzing without scoping | Irrelevant noise | Use target directories from parameters |
| Missing transitive dependencies | Incomplete picture | Trace full dependency chains |
| Ignoring type-only imports | Incomplete TypeScript analysis | Track separately with `import type` |
| Single-file focus | Misses architectural patterns | Analyze module boundaries |
| No file:line references | Unverifiable findings | Include source locations for all claims |

## Examples

**TypeScript Monorepo**: Glob `packages/*/src/**/*.ts`, parse 200 files. Build graph with 450 edges. Detect 3 cycles in shared utilities. Report: `packages/shared/utils.ts:15` imports `packages/core/helpers.ts:42` which imports back. Recommend extracting common interface.

**Python Package**: Glob `src/**/*.py`, analyze 80 files. Compute coupling: `src/api/routes.py` has Ce=25 (high). Parse `requirements.txt` for 45 external dependencies, 12 outdated. Report module coupling table and upgrade recommendations.

## Related

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallelization.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
