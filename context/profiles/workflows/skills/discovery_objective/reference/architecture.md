# Architecture Discovery Objective

Discover architectural patterns and component boundaries through systematic codebase analysis. Architecture discovery reveals design structures, layering schemes, dependency relationships, and violations of separation of concerns.

## Base Behavior

the `discovery_objective` skill

## Architecture Categories

### Structural Patterns

Architectural styles manifest through directory organization and import patterns. Layered architectures arrange modules into hierarchies (presentation, business, data) with unidirectional dependencies. Hexagonal architectures isolate core logic from external adapters via port interfaces. Identify structural patterns by examining directory trees and cross-module import relationships.

### Component Boundaries

Component boundaries define contracts between system parts. Examine interface definitions, dependency directions, and coupling levels. Check abstraction layers and their enforcement. Identify communication patterns: direct calls, events, or message passing. Well-defined boundaries show minimal cross-cutting imports and clear public APIs.

### Design Patterns

Catalog GoF patterns (Factory, Strategy, Observer, Singleton) and their implementation consistency. Use graph-based detection: build dependency graphs, traverse for subgraphs matching known structures. ML classifiers extract metrics from class hierarchies and method calls for pattern identification. Note pattern violations and inconsistent implementations.

### Dependency Structures

Map import and call relationships between modules. Build module dependency graphs with nodes (modules, classes) and edges (imports, calls). Detect circular dependencies via cycle detection algorithms. Calculate fan-in/fan-out metrics. Flag architectural drift indicators: violated layer boundaries, increasing coupling, cyclical dependencies.

## Discovery Methodology

### Phase 1: Broad Scan

Brief template for a procedure-depth agent (launched from objective depth):

```text
Perform a broad scan and categorize files by architectural layer or domain boundary.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`
```

**Architecture-specific application**: Categorize files by architectural layer (presentation, business, data) or domain boundary. Focus on layering clues in directory structure—nested controllers, services, repositories suggest layered organization. Count files per category to establish architectural scope.

### Phase 2: Pattern Detection

Brief template for a procedure-depth agent (launched from objective depth):

```text
Detect architectural patterns by building dependency matrices from import patterns across modules.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/pattern_detection.md`
```

**Architecture-specific application**: Build dependency matrices by grepping import patterns across modules. Upper-triangular dependency matrices indicate layered architecture compliance; lower-triangular entries reveal boundary violations. Map interface boundaries by comparing public API exports against internal module calls.

### Phase 3: Boundary Validation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Validate component boundaries and dependency direction against architectural principles.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/boundary_validation.md`
```

**Architecture-specific application**: Verify dependency direction against architectural principles—upper layers depend on lower layers, not reverse. Check SOLID adherence at component boundaries: single responsibility per module, interface segregation at public APIs. Identify circular dependencies through import chain analysis. Flag high fan-out modules depending on many others as coupling risks.

### Phase 4: Architecture Cataloging

Document each component with: name, responsibility, public interfaces, and file boundaries. List dependencies (incoming and outgoing) with `file:line` references. Catalog patterns applied with concrete examples. Identify violations and anti-patterns with evidence. Include architectural drift indicators for ongoing monitoring.

## Tool Permissions

**Allowed**:
- Glob: File pattern matching for directory structure analysis
- Grep: Import pattern search, dependency chain tracing
- Read: Interface examination, component analysis
- Bash: Read-only git commands (log, diff for architectural evolution)
- Write: reserved report path only

**Forbidden**:
- Modifying source files
- Launching sub-agents
- Writing outside the reserved report path
- Making architectural decisions (discovery is read-only analysis)

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Organize and write the architecture discovery report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Architecture discovery reports include these objective-specific sections:

- **Architectural Layers**: Identified layers with responsibilities and `file:line` boundaries
- **Component Boundaries**: Component inventory (name, responsibility, interfaces, dependencies)
- **Dependency Analysis**: Import patterns, circular dependencies, coupling metrics with evidence
- **Design Patterns**: Patterns found with concrete examples and `file:line` references
- **Violations and Anti-Patterns**: Boundary breaches, SOLID violations, God Object, Big Ball of Mud

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Speculating without evidence | Unverifiable architecture claims | Reference all claims with `file:line` |
| Ignoring dependency direction | Misses architectural violations | Verify layer compliance systematically |
| Focusing on single files | Misses system-level patterns | Analyze relationships across modules |
| Confusing logical vs physical structure | Misleading architecture maps | Validate logical components against actual code |
| Documenting aspirational architecture | Fictional documentation | Ground findings in implementation |

## Examples

**Layered Architecture Discovery**: Analyze Express.js application for layer violations. Glob `src/**/*.js`, grep `require(` patterns from controllers to models. Report showing 3 layers (routes, services, data) with 2 violations where controllers bypass services directly.

**Microservice Boundary Detection**: Identify service boundaries in monorepo. Analyze import graphs, detect module clusters with minimal external dependencies. Report cataloging 4 candidate services with interface boundaries and shared dependencies.

**Anti-Pattern Inventory**: Search for God Object indicators (classes >500 lines, multiple unrelated responsibilities). Grep for high fan-out modules. Report listing 3 God Objects, 2 Big Ball of Mud clusters, with refactoring evidence.

## Related

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
