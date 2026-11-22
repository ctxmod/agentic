---
description: "Gather information through systematic exploration and produce a comprehensive discovery report at a report path reserved just-in-time via reserve_work_path(type=\"report\")."
binds_to_depth: objective
---

# Discovery Objective

Gather information through systematic exploration. Read files, search
patterns, catalog structures. Produce a comprehensive report at a
report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Variant Selection

Specialized discovery variants add specific methodology to this base
objective. Read the variant matching the assigned scope; variants are
additive, not replacements. Each variant corresponds to one of the
four discovery capability phases that dispatch this objective.

| Phase dispatching | Variant | Read |
|-------------------|---------|------|
| `discover_architecture_phase` | architecture | `reference/architecture.md` |
| `discover_consumers_phase` | consumers | `reference/consumers.md` |
| `discover_dependencies_phase` | dependencies | `reference/dependencies.md` |
| `discover_patterns_phase` | patterns | `reference/patterns.md` |
| (ad-hoc, any phase) | repo-source | `reference/repo-source.md` |
| (ad-hoc, any phase) | repo-docs | `reference/repo-docs.md` |
| (ad-hoc, any phase) | work-items | `reference/work-items.md` |
| (ad-hoc, any phase) | any-work-items | `reference/any-work-items.md` |
| (ad-hoc, any phase) | external | `reference/external.md` |

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_objective/reference/any_work_items.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_objective/reference/architecture.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_objective/reference/consumers.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_objective/reference/dependencies.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_objective/reference/external.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_objective/reference/patterns.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_objective/reference/repo_docs.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_objective/reference/repo_source.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_objective/reference/work_items.md" />

Variants must be read when even somewhat relevant.

- the /discover command selection guidance — which discovery variant
  fits the assigned scope — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/discover.md" />

## Core Behavior

1. **Explore**: Use Glob, Grep, and Read tools to gather information
2. **Organize**: Structure findings hierarchically by category
3. **Reference**: Include file paths with line ranges for all claims
4. **Report**: Write findings to the reserved report path
5. **Summarize**: Return brief summary (2-3 sentences) with report path

## Pre-Discovery

Before exploring:

1. Read any provided reference files for methodology guidance
2. Identify target directories and file patterns
3. Understand scoping constraints (if provided)
4. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

## During Discovery

While exploring:

- Use Glob for file pattern matching
- Use Grep for content search
- Use Read for file examination
- Reference files with line ranges, not inline content
- Summarize findings rather than listing exhaustively
- Identify gaps requiring further investigation

## Supporting Procedures

Discovery procedures carry the step-level mechanics. Link conditionally on
what the assigned scope requires:

the `discovery_procedure` skill

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Organize and write the discovery report at a report path reserved just-in-time via `ctxmod work new report "<title>"` following the standard report structure.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Discovery reports include these objective-specific sections beyond the
standard structure:

- **Findings**: Organized by category with file references
- **Notable Items**: Specific files or patterns warranting attention (include `file:line` ranges)
- **Gaps**: Areas requiring further investigation

Target length: 500-1500 words. Maximize signal-to-noise ratio.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_discovery.md" />

## Tool Constraints

**Allowed**:
- Glob: File pattern matching
- Grep: Content search
- Read: File examination
- Bash: Read-only commands (ls, git log, git diff)
- Write: Report file at the reserved report path only

**Forbidden**:
- Modifying source files
- Launching sub-agents
- Writing outside the reserved report path
- Making implementation decisions

## Response Format

After writing the report, return a brief response:

```
Wrote discovery report to <reserved report path>.

Summary: [2-3 sentence summary of key findings]
```

Do not return the full report content. The report file is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Returning full reports in response | Bloats orchestrator context | Return summary + path |
| Claims without file references | Unverifiable findings | Include `file:line` references |
| Exhaustive listing | Low signal, high noise | Summarize and categorize |
| Skipping broad scan | Miss context | Start with `**/*` patterns |
| Writing to arbitrary paths | Violates constraints | Write only to the reserved report path |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile

## Sub-Layer Launch

When this objective requires step-level execution, launch the
`discovery_procedure` agent. Procedure agents carry the per-procedure
mechanics under 
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/boundary_validation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/consumer_search.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/entry_point_detection.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/graph_construction.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/import_extraction.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/pattern_detection.md`
.

Brief template for the `discovery_procedure` agent — Execute <procedure-name> within discovery objective:

```text
<ctxmod_activate skill="discovery_procedure" />
Procedure: <procedure-name>
<procedure-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Available procedure agents in the discovery domain: dispatch against
the procedure variants in 
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/boundary_validation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/consumer_search.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/entry_point_detection.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/graph_construction.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/import_extraction.md`
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/pattern_detection.md`

(broad-scan, consumer-search, entry-point-detection, import-extraction,
graph-construction, pattern-detection, boundary-validation).

## Parent-Layer Dispatch

This objective is dispatched by four sibling phase skills in the
discovery capability, one per capability variant:

| Phase skill | Variant typically dispatched | Reference material |
|-------------|------------------------------|--------------------|
| `discover_architecture_phase` | architecture | `skills/discover-architecture-phase/reference/*.md` |
| `discover_consumers_phase` | consumers | `skills/discover-consumers-phase/reference/*.md` |
| `discover_dependencies_phase` | dependencies | `skills/discover-dependencies-phase/reference/*.md` |
| `discover_patterns_phase` | patterns | `skills/discover-patterns-phase/reference/*.md` |

Ad-hoc objective-depth agents may also load this skill for general
discovery work outside any of the four capability phase chains.
