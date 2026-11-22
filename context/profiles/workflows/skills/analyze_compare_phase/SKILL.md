---
description: "Comparative analysis phase — parallel discovery per alternative, criteria definition, parallel evaluation, synthesis with recommendation."
binds_to_depth: phase
---

# Analyze Compare Phase

Comparative analysis phase — evaluate multiple alternatives against
consistent criteria and produce decision support with trade-off
analysis and context-dependent recommendations. This is one of five
sibling phase skills in the analysis capability (compare, debt, gap,
quality, comprehensive); they share the same objective
(`analysis_objective`) and procedure (`analysis_procedure`) layers
and differ only in the reference material they dispatch against.

Comparative analysis is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/analyze_compare`
slash-command, or any other domain's workflow agent references this
phase as a "compare alternatives" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Evaluating multiple alternatives against consistent criteria
- Decision support requires systematic comparison
- Trade-offs need explicit documentation
- Context-dependent recommendations guide selection

Comparative analysis produces decision support, not decisions.
Recommendation includes conditions and trade-offs.

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a four-step
fan-out fan-in sequence. The same objective (`analysis_objective`)
runs each time; the phase-specific reference material below scopes
each dispatch.

```
Phase 1: Discovery (Parallel)
         ├── Alternative A discovery
         ├── Alternative B discovery
         └── Alternative C discovery
         ↓
Phase 2: Criteria Definition
         ↓
Phase 3: Evaluation (Parallel)
         ├── Alternative A evaluation
         ├── Alternative B evaluation
         └── Alternative C evaluation
         ↓
Phase 4: Synthesis (Fan-In)
```

### Phase 1: Discovery (Parallel)

Launch one discovery sub-agent per alternative in a single message.
Each works on independent scope.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_phase_selection.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery-{alt-slug}.md` (parallel)

### Phase 2: Criteria Definition

Sequential pattern following parallel discovery. Derive criteria from
discovered characteristics; ensure criteria are measurable and
weighted; include both functional and non-functional criteria.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_general.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-criteria.md`

### Phase 3: Evaluation (Parallel)

Launch one evaluation sub-agent per alternative in a single message.
Each evaluates against the same criteria; score each criterion with
documented evidence; note confidence level per criterion.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analyze_compare_phase/reference/compare.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-evaluation-{alt-slug}.md` (parallel)

### Phase 4: Synthesis (Fan-In)

Fan-in pattern aggregating all evaluations. Create comparison matrix,
identify trade-offs, recommend with rationale and conditions.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_synthesis.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-comparison.md`

## Information Flow

```
Phase 1: Discovery
- Receives: {inherited work item context}, alternatives list
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery-{alt-slug}.md (parallel)

Phase 2: Criteria Definition
- Receives: {inherited}, Phase 1 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-criteria.md

Phase 3: Evaluation
- Receives: {inherited}, Phase 1 + Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-evaluation-{alt-slug}.md (parallel)

Phase 4: Synthesis
- Receives: {inherited}, all previous phase outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-comparison.md
```

Main agent passes report **paths** between phases, never report
content. Phase 4 reads prior phase report paths as INPUT_REPORTs.

## Variants

**Criteria-Driven Comparison**: When evaluation criteria provided
explicitly via `--criteria`, skip derivation:
`Discovery → Evaluation → Synthesis`.

**Two-Alternative Comparison**: For binary decisions with implicit
criteria: `Parallel Discovery → Parallel Evaluation → Synthesis`.

## Error Handling

| Situation | Response |
|-----------|----------|
| Alternatives not independent | Use sequential pattern instead of parallel |
| Criteria ambiguous | Request explicit criteria via `--criteria` |
| Evaluation scores lack evidence | Request re-evaluation with documentation |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Making decisions instead of recommendations | Provide decision support; authority remains with developers |
| Single criterion evaluation | Use multiple criteria for comprehensive comparison |
| Ignoring context-dependency | Document conditions that would change recommendation |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`analysis_objective` agent. The objective in turn launches
`analysis_procedure` for step mechanics.

Brief template for the `analysis_objective` agent — Execute <phase-name> within comparative analysis:

```text
<ctxmod_activate skill="analysis_objective" />
Variant: comparative
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (discovery) | dispatched via discovery_objective | broad-scan, entry-point-detection |
| Phase 2 (criteria) | comparative | criteria-weighting |
| Phase 3 (evaluation) | comparative | impact-scoring, confidence-assessment |
| Phase 4 (synthesis) | synthesis | pattern-aggregation, contradiction-resolution |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"compare alternatives" as a sub-step. Launch the
`analyze_compare_phase` agent from any workflow that needs
decision support.

Brief template for the `analyze_compare_phase` agent — Execute comparative analysis phase:

```text
<ctxmod_activate skill="analyze_compare_phase" />
Execute the full discovery → criteria → evaluation → synthesis chain.
```

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every analysis phase
variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_phase_selection.md" />

For a single-scope analysis that does not need the full four-phase
chain, dispatch the general phase template directly:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_general.md" />

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_analysis.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped analysis guidance for the sub-agents this phase launches:

the `analysis_objective` skill
the `analysis_procedure` skill
