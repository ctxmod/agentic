# Boundary Validation Procedure

Verify architectural compliance through dependency direction analysis and layer boundary enforcement checks.

## Prerequisites

- Component or layer identification completed (via broad scan or pattern detection)
- Import/call dependency patterns mapped across modules
- Expected architecture direction established (upper → lower layers)

## Steps

1. **Construct dependency matrix** mapping relationships between layers or components. Rows represent consuming modules, columns represent dependencies. Mark cell (i,j) if module i depends on module j.

2. **Check dependency direction** by examining matrix lower-triangular entries. For layered architectures, upper layers should depend on lower layers only. Lower-triangular entries indicate upward dependencies (violations).

3. **Trace import chains** for suspicious dependencies. Use Grep with import patterns to follow cross-boundary references. Record file:line evidence: `src/controllers/user.js:15` imports from `src/models/database.js`.

4. **Detect circular dependencies** through cycle identification in dependency graphs. Follow import chains from module A → B → C → A patterns. Document circular reference locations.

5. **Calculate fan-out metrics** per module. Count outgoing dependencies (modules that depend on others). Flag modules with fan-out >7 as high coupling candidates.

6. **Verify SOLID compliance** at boundaries:
   - **Single Responsibility**: Check if modules handle multiple unrelated concerns
   - **Interface Segregation**: Examine if clients depend on unused interface methods
   - **Dependency Inversion**: Verify high-level modules depend on abstractions, not concrete implementations

## Decision Points

**No clear layers exist**: Treat system as component-based architecture. Verify component boundaries instead of layer direction. Focus on circular dependencies and coupling metrics.

**Mixed architectural styles**: Apply validation per subsystem. Layered sections use direction checking; component sections use coupling analysis and circular dependency detection.

**Dependency inversion violations present**: Prioritize these over direction violations. Inverted dependencies (lower layers importing upper layers) represent more serious architectural violations than cross-layer calls.

**High coupling everywhere**: This indicates "Big Ball of Mud" anti-pattern. Shift focus to identifying potential refactoring boundaries rather than boundary enforcement.

## Success Criteria

- Dependency matrix constructed with all cross-module relationships mapped
- Lower-triangular violations documented with file:line evidence
- Circular dependencies cataloged with import chain traces
- High fan-out modules identified (>7 outgoing dependencies)
- SOLID violations recorded at boundaries with specific examples

## Common Issues

**Transitive dependencies obscure direction**: Indirect dependencies create false violation signals. Trace full import chains to distinguish direct violations from permissible indirect relationships.

**Test files violate layer boundaries**: Test code often depends on multiple layers. Exclude test directories from boundary validation or document test-specific exceptions.

**Shared utilities create apparent violations**: Common modules imported across layers (helpers, constants) appear as violations. Document shared dependencies as architectural exceptions rather than violations.

**Dynamic imports invisible to static analysis**: Lazy-loaded or runtime imports require execution tracing. Note this limitation in findings; flag suspicious modules requiring runtime verification.

**Cyclic dependencies through factories**: Factories and dependency injection containers create dependency cycles that may be intentional. Evaluate context before flagging as violations; document architectural justification when present.
