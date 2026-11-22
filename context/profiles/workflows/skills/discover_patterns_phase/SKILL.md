---
description: "Pattern discovery phase — inventory artifacts, extract patterns in parallel, synthesize pattern findings."
binds_to_depth: phase
---

# Discover Patterns Phase

Pattern discovery phase — identify recurring patterns, conventions,
and idioms within artifacts. This is one of four sibling phase skills
in the discovery capability (architecture, consumers, dependencies,
patterns); they share the same objective (`discovery_objective`) and
procedure (`discovery_procedure`) layers and differ only in the
reference material they dispatch against.

Pattern discovery is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/discover_patterns`
slash-command, or any other domain's workflow agent references this
phase as a "discover patterns" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Identifying recurring conventions and idioms
- Understanding naming, structural, and organizational patterns
- Establishing baseline for consistency validation
- Documenting implicit standards for onboarding
- Preparing for standardization efforts

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a three-step
sequence. The same objective (`discovery_objective`) runs each time;
the phase-specific reference material below scopes each dispatch.

```
Phase 1: Artifact Inventory (sequential)
         ↓
Phase 2: Pattern Extraction (parallel)
         ↓
Phase 3: Synthesis (sequential)
```

### Phase 1: Artifact Inventory

Dispatch `discovery_objective` to catalog artifacts for pattern
analysis. Inventory only; do not extract patterns yet.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_patterns_phase/reference/artifact_inventory.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-inventory.md`

### Phase 2: Pattern Extraction (Parallel)

Launch all three pattern extraction sub-agents in parallel in a single
message. Each focuses on a different pattern category.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_patterns_phase/reference/structural_patterns.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_patterns_phase/reference/naming_patterns.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_patterns_phase/reference/organizational_patterns.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-structural-patterns.md`,
  `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-naming-patterns.md`,
  `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-organizational-patterns.md`

### Phase 3: Synthesis

Dispatch `discovery_objective` to synthesize pattern findings into a
consolidated report. Final phase; produces consolidated pattern
report. Synthesize findings only; do not recommend changes.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discover_patterns_phase/reference/pattern_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-patterns.md`

## Information Flow

```
Phase 1: Artifact Inventory
- Receives: {inherited work item context}
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-inventory.md

Phase 2: Pattern Extraction
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-structural-patterns.md,
            {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-naming-patterns.md,
            {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-organizational-patterns.md

Phase 3: Synthesis
- Receives: {inherited}, Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-patterns.md
```

Main agent passes report **paths** between phases, never report
content. Phase 3 reads the three Phase 2 report paths as INPUT_REPORTs.

## Execution Strategy

1. **Plan**: Define scope and pattern type filters
2. **Artifact Inventory**: Launch inventory sub-agent; wait for
   completion
3. **Pattern Extraction**: Launch parallel pattern extraction sub-agents
   in single message; wait for completion
4. **Synthesis**: Launch synthesis sub-agent with Phase 2 outputs; wait
   for completion
5. **Complete**: Return pattern summary and report path

## Parameters

| Parameter | Values | Description |
|-----------|--------|-------------|
| `--type` | `structural`, `naming`, `organizational`, `behavioral`, `all` | Pattern type filter (default: `all`) |

## Error Handling

| Situation | Response |
|-----------|----------|
| Empty inventory | Report and exit |
| No patterns detected | Document absence, exit |
| Synthesis cannot consolidate | Report conflicting findings, exit |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Extracting patterns before inventory | Inventory first |
| Sequential pattern extraction | Launch all three extraction agents in parallel |
| Recommending changes during synthesis | Synthesize findings only |
| Inlining report content | Pass report paths between phases |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`discovery_objective` agent. The objective in turn launches
`discovery_procedure` for step mechanics.

Brief template for the `discovery_objective` agent — Execute <phase-name> within pattern discovery:

```text
<ctxmod_activate skill="agentic_workflows_discovery_objective" />
Variant: patterns
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (inventory) | patterns (inventory scope) | broad-scan |
| Phase 2 (extraction) | patterns (extraction scope) | pattern-detection |
| Phase 3 (synthesis) | patterns (synthesis scope) | pattern-detection |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"discover patterns" as a sub-step. Launch the
`discover_patterns_phase` agent from any workflow that needs pattern
identification.

Brief template for the `discover_patterns_phase` agent — Execute pattern discovery phase:

```text
<ctxmod_activate skill="agentic_workflows_discover_patterns_phase" />
Execute the full inventory → extract → synthesis chain.
```

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every discovery phase
variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_phase_selection.md" />

For a single-scope discovery that does not need the full three-phase
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
