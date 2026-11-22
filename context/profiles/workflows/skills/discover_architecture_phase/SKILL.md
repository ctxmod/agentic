---
description: "Architecture discovery phase — scan structure, identify boundaries, map layers, synthesize architectural findings."
binds_to_depth: phase
---

# Discover Architecture Phase

Architecture discovery phase — map artifact structure, organizational
boundaries, and component relationships. This is one of four sibling
phase skills in the discovery capability (architecture, consumers,
dependencies, patterns); they share the same objective
(`discovery_objective`) and procedure (`discovery_procedure`) layers
and differ only in the reference material they dispatch against.

Architecture discovery is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/discover_architecture`
slash-command, or any other domain's workflow agent references this
phase as a "discover architecture" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Understanding structural organization of a scope
- Identifying component boundaries and interfaces
- Mapping architectural layers and relationships
- Documenting organizational patterns
- Establishing foundational context for planning

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a four-step
sequence. The same objective (`discovery_objective`) runs each time;
the phase-specific reference material below scopes each dispatch.

```
Phase 1: Broad Scan (sequential)
         ↓
Phase 2: Boundary Detection (parallel)
         ↓
Phase 3: Layer Mapping (sequential)
         ↓
Phase 4: Synthesis (sequential)
```

### Phase 1: Broad Scan

Dispatch `discovery_objective` to scan structure and identify
organizational patterns. Scan only; do not interpret significance yet.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_architecture_phase/reference/scan.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-scan.md`

### Phase 2: Boundary Detection (Parallel)

Launch both boundary detection sub-agents in parallel in a single
message. Each focuses on different boundary types.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_architecture_phase/reference/boundary_detection.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_architecture_phase/reference/config_boundaries.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-boundaries.md`,
  `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-config-boundaries.md`

### Phase 3: Layer Mapping

Dispatch `discovery_objective` to map architectural layers and
inter-layer relationships. Consumes outputs from both boundary
detection sub-agents.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_architecture_phase/reference/layer_mapping.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-layers.md`

### Phase 4: Synthesis

Dispatch `discovery_objective` to synthesize architecture findings into
a consolidated report. Final phase; produces consolidated architecture
report. Synthesize findings only; do not recommend changes.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_architecture_phase/reference/architecture_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-architecture.md`

## Information Flow

```
Phase 1: Broad Scan
- Receives: {inherited work item context}
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-scan.md

Phase 2: Boundary Detection
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-boundaries.md,
            {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-config-boundaries.md

Phase 3: Layer Mapping
- Receives: {inherited}, Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-layers.md

Phase 4: Synthesis
- Receives: {inherited}, all previous phase outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-architecture.md
```

Main agent passes report **paths** between phases, never report
content. Phase 4 reads prior phase report paths as INPUT_REPORTs.

## Execution Strategy

1. **Plan**: Define scope and objectives
2. **Broad Scan**: Launch scan sub-agent; wait for completion
3. **Boundary Detection**: Launch parallel boundary sub-agents in single
   message; wait for completion
4. **Layer Mapping**: Launch layer mapping sub-agent with Phase 2
   outputs; wait for completion
5. **Synthesis**: Launch synthesis sub-agent with all phase outputs;
   wait for completion
6. **Complete**: Return architecture summary and report path

## Error Handling

| Situation | Response |
|-----------|----------|
| Empty scope (no files match) | Report and exit |
| Phase produces no findings | Document gap, continue to next phase |
| Synthesis cannot consolidate | Report conflicting findings, exit |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Interpreting significance during scan | Scan only in Phase 1 |
| Sequential boundary detection | Launch boundary agents in parallel |
| Recommending changes during synthesis | Synthesize findings only |
| Inlining report content | Pass report paths between phases |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`discovery_objective` agent. The objective in turn launches
`discovery_procedure` for step mechanics.

Brief template for the `discovery_objective` agent — Execute <phase-name> within architecture discovery:

```text
<ctxmod_activate skill="discovery_objective" />
Variant: architecture
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (scan) | architecture (scan scope) | broad-scan |
| Phase 2 (boundaries) | architecture (boundary scope) | entry-point-detection, boundary-validation |
| Phase 3 (layers) | architecture (layer scope) | graph-construction, boundary-validation |
| Phase 4 (synthesis) | architecture (synthesis scope) | pattern-detection |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"discover architecture" as a sub-step. Launch the
`discover_architecture_phase` agent from any workflow that needs
structural context.

Brief template for the `discover_architecture_phase` agent — Execute architecture discovery phase:

```text
<ctxmod_activate skill="discover_architecture_phase" />
Execute the full scan → boundaries → layers → synthesis chain.
```

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every discovery phase
variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_phase_selection.md" />

For a single-scope discovery that does not need the full four-phase
chain, dispatch the general phase template directly:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_general.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_multi_source.md" />

Before launching sub-agents, run the preflight reference-discovery
phase to build targeted compulsory reads per sub-agent:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_workflow_reference.md" />

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallelization.md" />
`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_discovery.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped discovery guidance for the sub-agents this phase launches:

the `discovery_objective` skill
the `discovery_procedure` skill
