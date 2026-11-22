---
description: "Dependency discovery phase — identify components, extract dependencies in parallel, construct graph, synthesize dependency findings."
binds_to_depth: phase
---

# Discover Dependencies Phase

Dependency discovery phase — map dependencies, relationships, and
coupling between components. This is one of four sibling phase skills
in the discovery capability (architecture, consumers, dependencies,
patterns); they share the same objective (`discovery_objective`) and
procedure (`discovery_procedure`) layers and differ only in the
reference material they dispatch against.

Dependency discovery is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/discover_dependencies`
slash-command, or any other domain's workflow agent references this
phase as a "discover dependencies" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Understanding component relationships and coupling
- Identifying declared, internal, and implicit dependencies
- Building dependency graphs for impact analysis
- Planning changes to shared components
- Analyzing dependency health and risks

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a four-step
sequence. The same objective (`discovery_objective`) runs each time;
the phase-specific reference material below scopes each dispatch.

```
Phase 1: Component Identification (sequential)
         ↓
Phase 2: Dependency Extraction (parallel)
         ↓
Phase 3: Graph Construction (sequential)
         ↓
Phase 4: Synthesis (sequential)
```

### Phase 1: Component Identification

Dispatch `discovery_objective` to identify components and their
boundaries. Identify components only; do not map dependencies yet.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_dependencies_phase/reference/component_identification.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-components.md`

### Phase 2: Dependency Extraction (Parallel)

Launch all three dependency extraction sub-agents in parallel in a
single message. Each focuses on a different dependency source.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_dependencies_phase/reference/declared_dependencies.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_dependencies_phase/reference/internal_dependencies.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_dependencies_phase/reference/implicit_dependencies.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-declared-deps.md`,
  `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-internal-deps.md`,
  `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-implicit-deps.md`

### Phase 3: Graph Construction

Dispatch `discovery_objective` to merge all dependency sources into a
unified graph. Consumes outputs from all three extraction sub-agents.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_dependencies_phase/reference/dependency_graph.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-dependency-graph.md`

### Phase 4: Synthesis

Dispatch `discovery_objective` to synthesize dependency findings into a
consolidated report. Final phase; produces consolidated dependency
report. Synthesize findings only; do not recommend changes.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_dependencies_phase/reference/dependency_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-dependencies.md`

## Information Flow

```
Phase 1: Component Identification
- Receives: {inherited work item context}
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-components.md

Phase 2: Dependency Extraction
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-declared-deps.md,
            {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-internal-deps.md,
            {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-implicit-deps.md

Phase 3: Graph Construction
- Receives: {inherited}, Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-dependency-graph.md

Phase 4: Synthesis
- Receives: {inherited}, all previous phase outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-dependencies.md
```

Main agent passes report **paths** between phases, never report
content. Phase 3 reads the three Phase 2 report paths as INPUT_REPORTs.

## Execution Strategy

1. **Plan**: Define scope and depth parameters
2. **Component Identification**: Launch component identification
   sub-agent; wait for completion
3. **Dependency Extraction**: Launch parallel dependency extraction
   sub-agents in single message; wait for completion
4. **Graph Construction**: Launch graph construction sub-agent with
   Phase 2 outputs; wait for completion
5. **Synthesis**: Launch synthesis sub-agent with all phase outputs;
   wait for completion
6. **Complete**: Return dependency summary and report path

## Parameters

| Parameter | Values | Description |
|-----------|--------|-------------|
| `--depth` | `direct`, `transitive`, `all` | Dependency depth to analyze (default: `direct`) |

## Error Handling

| Situation | Response |
|-----------|----------|
| No components identified | Report and exit |
| Manifest files missing | Document gap, proceed with internal + implicit |
| Graph construction finds cycles | Document cycle, continue |
| Synthesis cannot consolidate | Report conflicting findings, exit |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Mapping dependencies before identifying components | Identify components first |
| Sequential dependency extraction | Launch all three extraction agents in parallel |
| Recommending changes during synthesis | Synthesize findings only |
| Inlining report content | Pass report paths between phases |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`discovery_objective` agent. The objective in turn launches
`discovery_procedure` for step mechanics.

Brief template for the `discovery_objective` agent — Execute <phase-name> within dependency discovery:

```text
<ctxmod_activate skill="discovery_objective" />
Variant: dependencies
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (components) | dependencies (component scope) | entry-point-detection |
| Phase 2 (extraction) | dependencies (extraction scope) | import-extraction, consumer-search |
| Phase 3 (graph) | dependencies (graph scope) | graph-construction |
| Phase 4 (synthesis) | dependencies (synthesis scope) | pattern-detection, boundary-validation |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"discover dependencies" as a sub-step. Launch the
`discover_dependencies_phase` agent from any workflow that needs
dependency mapping.

Brief template for the `discover_dependencies_phase` agent — Execute dependency discovery phase:

```text
<ctxmod_activate skill="discover_dependencies_phase" />
Execute the full components → extract → graph → synthesis chain.
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
