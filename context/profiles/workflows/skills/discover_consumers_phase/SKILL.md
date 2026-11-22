---
description: "Consumer discovery phase — resolve target, search references in parallel, classify consumers, synthesize consumer findings."
binds_to_depth: phase
---

# Discover Consumers Phase

Consumer discovery phase — find all references, usages, and
dependencies on target artifacts. This is one of four sibling phase
skills in the discovery capability (architecture, consumers,
dependencies, patterns); they share the same objective
(`discovery_objective`) and procedure (`discovery_procedure`) layers
and differ only in the reference material they dispatch against.

Consumer discovery is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/discover_consumers`
slash-command, or any other domain's workflow agent references this
phase as a "discover consumers" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Finding all references to a target artifact
- Understanding impact of changes to shared elements
- Identifying tightly coupled consumers
- Planning migrations or refactoring efforts
- Analyzing artifact usage patterns

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a four-step
sequence. The same objective (`discovery_objective`) runs each time;
the phase-specific reference material below scopes each dispatch.

```
Phase 1: Target Resolution (sequential)
         ↓
Phase 2: Reference Search (parallel)
         ↓
Phase 3: Consumer Classification (sequential)
         ↓
Phase 4: Synthesis (sequential)
```

### Phase 1: Target Resolution

Dispatch `discovery_objective` to resolve target identity. Resolve
target identity only; do not search for consumers yet.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_consumers_phase/reference/target_resolution.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-target-resolution.md`

### Phase 2: Reference Search (Parallel)

Launch all three reference search sub-agents in parallel in a single
message. Each focuses on a different reference type.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_consumers_phase/reference/direct_references.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_consumers_phase/reference/indirect_references.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_consumers_phase/reference/config_references.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-direct-refs.md`,
  `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-indirect-refs.md`,
  `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-config-refs.md`

### Phase 3: Consumer Classification

Dispatch `discovery_objective` to classify consumers by relationship
type and criticality. Consumes outputs from all three reference search
sub-agents.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_consumers_phase/reference/consumer_classification.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-consumer-classification.md`

### Phase 4: Synthesis

Dispatch `discovery_objective` to synthesize consumer findings into a
consolidated report. Final phase; produces consolidated consumer
report. Synthesize findings only; do not recommend changes.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_consumers_phase/reference/consumer_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-consumers.md`

## Information Flow

```
Phase 1: Target Resolution
- Receives: {inherited work item context}, TARGET
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-target-resolution.md

Phase 2: Reference Search
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-direct-refs.md,
            {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-indirect-refs.md,
            {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-config-refs.md

Phase 3: Consumer Classification
- Receives: {inherited}, Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-consumer-classification.md

Phase 4: Synthesis
- Receives: {inherited}, all previous phase outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-consumers.md
```

Main agent passes report **paths** between phases, never report
content. Phase 3 reads the three Phase 2 report paths as INPUT_REPORTs.

## Execution Strategy

1. **Plan**: Define target and search scope
2. **Target Resolution**: Launch target resolution sub-agent; wait for
   completion
3. **Reference Search**: Launch parallel reference search sub-agents in
   single message; wait for completion
4. **Consumer Classification**: Launch classification sub-agent with
   Phase 2 outputs; wait for completion
5. **Synthesis**: Launch synthesis sub-agent with all phase outputs;
   wait for completion
6. **Complete**: Return consumer summary and report path

## Error Handling

| Situation | Response |
|-----------|----------|
| Target cannot be resolved | Report and exit |
| No references found | Document zero-consumer state, exit |
| Phase produces no findings | Document gap, continue to next phase |
| Synthesis cannot consolidate | Report conflicting findings, exit |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Searching for consumers before resolving target | Resolve target first |
| Sequential reference search | Launch all three search agents in parallel |
| Recommending changes during synthesis | Synthesize findings only |
| Inlining report content | Pass report paths between phases |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`discovery_objective` agent. The objective in turn launches
`discovery_procedure` for step mechanics.

Brief template for the `discovery_objective` agent — Execute <phase-name> within consumer discovery:

```text
<ctxmod_activate skill="agentic_workflows_discovery_objective" />
Variant: consumers
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (target resolution) | consumers (target scope) | entry-point-detection |
| Phase 2 (reference search) | consumers (reference scope) | consumer-search, import-extraction |
| Phase 3 (classification) | consumers (classification scope) | pattern-detection |
| Phase 4 (synthesis) | consumers (synthesis scope) | pattern-detection |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"discover consumers" as a sub-step. Launch the
`discover_consumers_phase` agent from any workflow that needs consumer
impact analysis.

Brief template for the `discover_consumers_phase` agent — Execute consumer discovery phase:

```text
<ctxmod_activate skill="agentic_workflows_discover_consumers_phase" />
Execute the full target → search → classify → synthesis chain.
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
