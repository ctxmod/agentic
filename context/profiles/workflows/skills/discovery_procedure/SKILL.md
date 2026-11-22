---
description: "Step-level discovery mechanics — broad scanning, consumer search, entry-point detection, import extraction, graph construction, pattern detection, and boundary validation."
binds_to_depth: procedure
---

# Discovery Procedures

Execute a single, bounded discovery procedure and return its result. Each
procedure below is a self-contained set of prerequisites, steps, and
decision points; the variant material lives in this skill's `reference/`
directory and is read conditionally based on what the assigned task needs.

## Procedure Selection

Read the reference file matching the assigned task. Do not read all seven —
each is independently applicable and reading extras wastes context.

| Assigned task | Read |
|---------------|------|
| Inventory a directory tree; categorize and count files before deeper work | `reference/broad_scan.md` |
| Find every file that references, imports, or depends on a target | `reference/consumer_search.md` |
| Identify the public interfaces external consumers or runtimes invoke | `reference/entry_point_detection.md` |
| Extract import/dependency statements from source files | `reference/import_extraction.md` |
| Build a directed graph from extracted import relationships | `reference/graph_construction.md` |
| Detect recurring structures via frequency and similarity analysis | `reference/pattern_detection.md` |
| Verify dependency direction and layer-boundary compliance | `reference/boundary_validation.md` |

### Broad Scan

Glob a target tree with recursive patterns, categorize the returned paths
(by type, module, or layer), count per category, and report the inventory
with directory-structure observations.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure/reference/broad_scan.md" />

### Consumer Search

Systematically identify files that reference, import, or depend on a
target to assess change impact and understand coupling patterns.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure/reference/consumer_search.md" />

### Entry Point Detection

Identify public interfaces that external consumers or runtime environments
invoke. Entry points reveal the intended usage surface, distinguishing
public APIs from internal implementation.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure/reference/entry_point_detection.md" />

### Import Extraction

Extract import and dependency statements from source files using
language-specific parsing patterns. Produces the raw import data that
graph construction and dependency analysis consume.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure/reference/import_extraction.md" />

### Graph Construction

Build directed graphs from extracted import relationships to enable
dependency analysis, cycle detection, and coupling metric computation.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure/reference/graph_construction.md" />

Runs after import extraction; the two are a pair.

### Pattern Detection

Identify recurring structures across a file corpus through frequency
analysis and similarity grouping, combining quantitative metrics with
qualitative similarity assessment.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure/reference/pattern_detection.md" />

### Boundary Validation

Verify architectural compliance through dependency direction analysis and
layer boundary enforcement checks.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure/reference/boundary_validation.md" />

Runs after graph construction when the discovery scope includes layering
compliance.

## Execution Discipline

- Execute only the assigned procedure; do not expand into adjacent steps.
- Report results to the caller-provided path; return a summary, not the
  full body.
- Reference findings with `file:line` ranges rather than inlining content.
- Note explicitly when a procedure's prerequisites are unmet rather than
  improvising a substitute.

## Objective Context

The objective these procedures serve, and its per-scope variants:

the `discovery_objective` skill

## Parent-Layer Dispatch

This procedure is dispatched by the shared `discovery_objective`,
which in turn is dispatched by four sibling phase skills in the
discovery capability:

| Phase skill | Procedures typically dispatched |
|-------------|---------------------------------|
| `discover_architecture_phase` | broad-scan, entry-point-detection, boundary-validation |
| `discover_consumers_phase` | consumer-search, entry-point-detection, import-extraction |
| `discover_dependencies_phase` | import-extraction, graph-construction, boundary-validation |
| `discover_patterns_phase` | broad-scan, pattern-detection |

Ad-hoc procedure-depth agents may also load this skill for general
discovery step mechanics outside any of the four capability phase
chains.

- the /discover command selection guidance — which discovery variant
  fits the assigned scope — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/discover.md" />

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
