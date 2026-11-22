---
description: "Comparative research phase — criteria definition, parallel per-alternative research, comparative analysis, recommendation."
binds_to_depth: phase
---

# Research Compare Phase

Comparative research phase — research and compare alternatives against
specified criteria for informed decision-making. This is one of three
sibling phase skills in the research capability (academic, compare,
practices); they share the general procedure layer (`research_procedure`)
and differ in scope and reference material.

Comparative research is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/research_compare`
slash-command, or any other domain's workflow agent references this
phase as a "compare alternatives" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/research_briefs.md" />

## When to Use

Use this phase when:

- Evaluating multiple alternatives for decision-making
- Selecting technologies, frameworks, or approaches
- Comparing solutions against defined criteria
- Generating evidence-based recommendations
- Documenting trade-offs and decision rationale

## Phase Orchestration

This phase dispatches procedure-depth sub-agents in a four-step
sequence. Comparative research uses the fan-out fan-in pattern with
independent research per alternative followed by synthesis.

```
Phase 1: Criteria Definition (sequential)
         ↓
Phase 2: Alternative Research (parallel)
         ↓
Phase 3: Comparative Analysis (sequential)
         ↓
Phase 4: Recommendation (sequential)
```

### Phase 1: Criteria Definition

Dispatch `research_procedure` to define evaluation criteria. Criteria
definition only; do not research alternatives yet. Generate 5-10
evaluation criteria with clear definitions.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_compare_phase/reference/criteria_definition.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-criteria-definition.md`

### Phase 2: Alternative Research (Parallel)

Launch one sub-agent per alternative in parallel in a single message.
Each agent researches one alternative against defined criteria. Do not
compare alternatives within individual research agents.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_compare_phase/reference/alternative_research.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-{alternative-slug}.md`
  (one per alternative)

### Phase 3: Comparative Analysis

Dispatch the analysis procedure. Consumes all alternative research
outputs. Creates comparison matrix for quantifiable criteria. Documents
trade-offs between alternatives.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_comparative.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-comparative-analysis.md`

### Phase 4: Recommendation

Dispatch the recommendation procedure. Final phase; generates
scenario-based recommendations. Include confidence levels for each
recommendation. Provide migration or adoption considerations.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_recommendation.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-recommendations.md`

## Information Flow

```
Phase 1: Criteria Definition
- Receives: {inherited work item context}, ALTERNATIVES, CRITERIA
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-criteria-definition.md

Phase 2: Alternative Research
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-{alternative-slug}.md
            (one per alternative)

Phase 3: Comparative Analysis
- Receives: {inherited}, Phase 1 output, Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-comparative-analysis.md

Phase 4: Recommendation
- Receives: {inherited}, Phase 3 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-recommendations.md
```

Main agent passes report **paths** between phases, never report
content. Later phases read prior phase report paths as INPUT_REPORTs.

## Execution Strategy

1. **Plan**: Define alternatives and evaluation criteria
2. **Criteria Definition**: Launch criteria definition sub-agent; wait
   for completion
3. **Alternative Research**: Launch parallel research sub-agents (one
   per alternative) in single message; wait for completion
4. **Comparative Analysis**: Launch analysis sub-agent with Phase 2
   outputs; wait for completion
5. **Recommendation**: Launch recommendation sub-agent with Phase 3
   output; wait for completion
6. **Complete**: Return recommendation summary and report path

## Error Handling

| Situation | Response |
|-----------|----------|
| No alternatives provided | Report and exit |
| Alternative research produces no findings | Document gap, continue with remaining alternatives |
| Comparative analysis cannot consolidate | Report conflicting findings, exit |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Comparing within alternative research | Research each alternative independently |
| Sequential alternative research | Launch one agent per alternative in parallel |
| Skipping criteria definition | Define criteria before research |
| Inlining report content | Pass report paths between phases |

## Sub-Layer Launch

When this phase requires procedure-depth execution, launch the
`research_procedure` agent for general step mechanics (query
formulation, source selection, evidence extraction, source evaluation,
bias detection).

Brief template for the `research_procedure` agent — Execute <phase-name> within comparative research:

```text
<ctxmod_activate skill="research_procedure" />
Procedure: <procedure-name>
Phase-specific instructions:
  <phase-specific parameters>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch procedures per phase position:

| Phase | Procedure |
|-------|-----------|
| Phase 1 (criteria definition) | `reference/query-formulation.md` (general) |
| Phase 2 (alternative research) | `reference/source-selection.md` + `reference/evidence-extraction.md` |
| Phase 3 (comparative analysis) | (analysis-domain procedures) |
| Phase 4 (recommendation) | (analysis-domain procedures) |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"compare alternatives" as a sub-step. Launch the
`research_compare_phase` agent from any workflow that needs
evidence-based comparison.

Brief template for the `research_compare_phase` agent — Execute comparative research phase:

```text
<ctxmod_activate skill="research_compare_phase" />
Execute the full criteria → research → analysis → recommendation chain.
```

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every research phase
variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_phase_selection.md" />

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_discovery.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped research guidance for the sub-agents this phase launches:

the `research_procedure` skill
