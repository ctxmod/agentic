# Baseline Discovery

Use when planning transformations that must preserve behavior (refactoring, migration). Discovers current structure and consumer relationships in parallel to establish a baseline for equivalence verification.

Brief template for a sub-agent:

```text
Discover current structure for transformation baseline.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-baseline-structure" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={transformation scope}

Delimitations:
- Discovery only; do not modify artifacts
- Document structure comprehensively for comparison

Notes:
- Capture artifact organization and relationships
- Document naming conventions in use
- Note cross-references and dependencies
```

Brief template for a sub-agent:

```text
Discover consumers and dependents of transformation targets.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-baseline-consumers" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={transformation scope}

Delimitations:
- Focus on inbound references to targets
- Identify all consumers that may need updates

Notes:
- Catalog direct consumers
- Note transitive dependencies
- Identify external consumers if applicable
```
