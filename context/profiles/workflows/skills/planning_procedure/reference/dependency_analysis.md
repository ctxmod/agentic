# Dependency Analysis Procedure

Identify relationships between planned changes, classify dependency types, and derive execution order. This procedure produces a dependency map with critical path that determines which changes must precede others and which can proceed in parallel.

## Prerequisites

- Planned changes inventoried with targets and descriptions
- Access to structural information about target artifacts (references, consumers, shared state)
- Constraints and boundaries of planned work defined
- Analysis or discovery reports available for change rationale

## Steps

### 1. List Planned Elements

Enumerate every discrete change included in the plan. Each element should represent one logical unit of work with a clear target and outcome.

```markdown
| ID | Target | Change Type | Description |
|----|--------|-------------|-------------|
| E1 | {artifact-path} | Modify | {what changes} |
| E2 | {artifact-path} | Create | {what is added} |
| E3 | {artifact-path} | Delete | {what is removed} |
```

Assign stable identifiers (E1, E2, ...) for dependency references. Record change type: Create, Modify, Delete, Move, or Rename.

### 2. Identify Dependencies Between Elements

For each pair of elements, determine whether a dependency exists. Examine three categories:

**Data dependencies**: One element produces output that another element consumes. Element E2 requires a structure defined by E1 before it can reference that structure.

**Structural dependencies**: One element modifies a container or framework that another element relies on. Renaming a shared component affects all elements that reference it.

**Ordering dependencies**: One element must complete first to avoid invalid intermediate states, even without direct data or structural coupling. Removing an old version before creating its replacement avoids ambiguity.

Record each dependency with its type:

```markdown
| From | To | Type | Rationale |
|------|-----|------|-----------|
| E1 | E2 | Data | E2 references structure created by E1 |
| E1 | E3 | Structural | E3 assumes container modified by E1 |
| E2 | E4 | Ordering | E4 validates output of E2 |
```

"From" must complete before "To" can begin.

### 3. Map Dependency Graph

Arrange elements into a directed acyclic graph based on identified dependencies. Each element is a node; each dependency is a directed edge.

```
E1 → E2 → E4
 ↘ E3
E5 (independent)
```

Verify the graph contains no cycles. A cycle indicates that two or more elements require each other to complete first, which prevents valid sequencing.

### 4. Identify Critical Path

The critical path is the longest chain of dependent elements measured by estimated effort. It determines the minimum total duration when parallel execution is available.

For each path through the graph:

1. Sum estimated effort across all elements in the path
2. Record the path with highest total
3. Mark elements on this path as schedule-sensitive

Elements not on the critical path have scheduling flexibility. They can shift within their dependency constraints without affecting total duration.

### 5. Determine Execution Order

Derive a valid execution sequence using topological ordering:

1. **Root elements**: No incoming dependencies. Execute first.
2. **Dependent elements**: Execute after all predecessors complete.
3. **Parallel groups**: Independent elements at the same depth can execute simultaneously.

```markdown
## Execution Order

### Group 1 (parallel)
- E1: {description} (root, no dependencies)
- E5: {description} (root, no dependencies)

### Group 2 (parallel, after Group 1)
- E2: {description} (depends on E1)
- E3: {description} (depends on E1)

### Group 3 (after Group 2)
- E4: {description} (depends on E2)
```

### 6. Document Assumptions

Record conditions assumed true during dependency analysis:

- State assumptions about artifact availability and accessibility
- Note assumed stability of external dependencies
- Document assumed ordering of concurrent external changes
- Flag dependencies inferred rather than verified

Each assumption represents a risk if invalidated during execution. The risk assessment procedure evaluates these assumptions when applicable.

## Decision Points

**Circular dependency detected**: Two or more elements form a cycle where each depends on another. Break the cycle by extracting a shared element, combining coupled elements into a single atomic unit, or introducing an intermediate artifact that decouples the relationship.

**Dependency information unavailable**: When structural relationships between artifacts are unclear, treat the relationship as a dependency. False dependencies add unnecessary ordering constraints but remain safe. Missed dependencies cause execution failures.

**External dependency identified**: An element depends on something outside the planned work scope -- a separate team's output, an external service, or an uncontrolled timeline. Document the external dependency explicitly, define a verification step to confirm readiness, and identify a contingency if the external dependency is delayed.

**Large number of elements**: When the plan contains more than 15-20 elements, group related elements into clusters. Analyze dependencies between clusters first, then within each cluster. This two-level approach keeps the graph manageable.

**All elements appear independent**: Verify that no implicit dependencies exist through shared state, configuration, or runtime environment. If elements are genuinely independent, document that execution order is flexible and parallel execution is safe.

## Success Criteria

- Every planned element listed with stable identifier and description
- Dependencies identified and classified by type (data, structural, ordering)
- Dependency graph verified cycle-free
- Critical path identified with effort estimates
- Execution order derived with parallel groups where applicable
- Assumptions documented for downstream risk evaluation

## Common Issues

**Implicit dependencies**: Elements appear independent but share configuration, runtime state, or environmental assumptions. Trace relationships beyond direct references -- check for shared resources, naming conventions, and environmental preconditions.

**Over-constrained ordering**: Treating preferences as hard dependencies reduces parallelism and extends execution duration. Distinguish between "must precede" (true dependency) and "typically precedes" (convention). Only true dependencies belong in the graph.

**Granularity mismatch**: Elements defined at inconsistent levels of detail produce misleading dependency maps. A single coarse element with many dependents obscures the actual coupling. Decompose large elements until dependency relationships are specific.

**Stale dependency information**: Structural relationships change as artifacts evolve. Re-verify dependencies if significant time passes between analysis and execution, or if other changes affect the same artifacts.

**Critical path tunnel vision**: Focusing exclusively on critical-path elements while neglecting near-critical paths. A delay on a near-critical path can shift it to become the new critical path. Monitor elements within 10-20% of critical-path duration.
