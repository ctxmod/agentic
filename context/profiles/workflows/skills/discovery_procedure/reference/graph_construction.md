# Graph Construction Procedure

Build directed graphs from extracted import relationships to enable dependency analysis, cycle detection, and coupling metric computation. The graph represents structural connections between code elements.

## Prerequisites

1. **Completed import extraction**: All dependency statements parsed from source files
2. **Node identification**: Unique identifiers established for each file or module
3. **Metadata schema**: Defined structure for edge attributes (import type, source location)

## Steps

### 1. Initialize Graph Structure

Create a directed graph data structure with the following properties:

- **Nodes**: Represent files, modules, or packages based on analysis scope
- **Edges**: Represent import relationships between nodes
- **Direction**: Flows from dependent to dependency (importer → importee)
- **Metadata storage**: Attach attributes to each edge

### 2. Add Nodes

For each extracted import target, create a node if it does not exist:

- Code-level: One node per source file
- Module-level: One node per logical module or package
- Package-level: One node per external dependency

Assign stable identifiers to nodes (file paths, module names, package names).

### 3. Add Edges

For each extracted import relationship, create a directed edge:

```
dependent_file --[imports]--> dependency
```

Attach edge metadata:

| Attribute | Description | Values |
|-----------|-------------|--------|
| `type` | Import category | `value`, `type`, `dynamic` |
| `line` | Source location | Line number in file |
| `resolved` | Path resolution status | `true`, `false` |
| `external` | External package flag | `true`, `false` |

### 4. Validate Graph Integrity

Perform validation checks:

- **Orphan nodes**: Nodes with zero edges (unreferenced exports)
- **Self-loops**: Files importing themselves (usually errors)
- **Duplicate edges**: Multiple imports from same source to same target
- **Unresolved edges**: Imports that failed path resolution

Log validation issues as low-severity findings.

### 5. Apply Graph Algorithms

Execute analysis algorithms on the constructed graph:

**Cycle detection**:
- Perform depth-first search (DFS) traversal
- Track recursion stack to identify back-edges
- Extract full cycle paths for reporting
- Flag circular dependencies as high-severity issues

**Coupling metrics**:
- Compute **Afferent Coupling (Ca)**: Count incoming edges per node
- Compute **Efferent Coupling (Ce)**: Count outgoing edges per node
- Calculate **Instability (I)**: `I = Ce / (Ca + Ce)`
- Values: 0 = completely stable, 1 = completely unstable

**Critical path analysis**:
- Identify nodes with highest transitive dependent count
- Rank nodes by impact: changes affect many downstream modules
- Flag high-impact nodes as architectural stability risks

**Orphan detection**:
- Find unreferenced exports: nodes with Ca = 0
- Find unused imports: nodes that are imported but never referenced
- Report as potential dead code for removal

### 6. Prepare Graph for Analysis

Export graph in analysis-ready format:

- **Adjacency list**: For traversal algorithms
- **Matrix representation**: For coupling computation
- **Serialization**: JSON or GraphML for persistence and visualization

## Decision Points

**Granularity choice**: Analyze at file, module, or package level based on discovery scope. File-level for detailed refactoring, package-level for architecture validation.

**Cycle severity**: Not all cycles require immediate action. Evaluate cycle length and position in dependency hierarchy. Short cycles in utility modules may be acceptable; cycles spanning architectural boundaries require refactoring.

**Stability threshold**: Set instability limits based on project requirements. Critical infrastructure should target I < 0.3. Application code may tolerate I < 0.7.

## Success Criteria

- **Completeness**: All extracted imports represented as edges
- **Accuracy**: Edge directions and metadata match source code
- **Integrity**: No unresolved nodes or broken edges
- **Analyzability**: Graph supports all required algorithm operations
- **Performance**: Construction completes in acceptable time for codebase size

## Common Issues

| Issue | Cause | Resolution |
|-------|-------|------------|
| Duplicate edges | Multiple import statements from same source | Deduplicate or consolidate with count metadata |
| High memory usage | Analyzing large codebases at file level | Switch to module-level granularity |
| False cycles | Type-only imports creating circular references | Exclude type-only edges from cycle detection |
| Unresolved external packages | Missing or corrupted lockfile | Cross-reference with multiple package sources |
| Orphan false positives | Test files, entry points, or dynamically loaded modules | Whitelist known orphan patterns |

## Related

`ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/dependencies.md`
