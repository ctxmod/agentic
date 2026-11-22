---
description: "Best practices research phase — parallel search across web, community, and GitHub sources, synthesis of current industry practices."
binds_to_depth: phase
---

# Research Practices Phase

Best practices research phase — research current best practices for a
topic from web sources, documentation, and community knowledge. This
is one of three sibling phase skills in the research capability
(academic, compare, practices); they share the general procedure layer
(`research_procedure`) and differ in scope and reference material.

Best practices research is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/research_practices`
slash-command, or any other domain's workflow agent references this
phase as a "research current practices" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/research_briefs.md" />

## When to Use

Use this phase when:

- Researching current industry practices and patterns
- Gathering evidence from web sources and documentation
- Understanding community consensus and common approaches
- Identifying anti-patterns and pitfalls to avoid
- Informing practical implementation decisions

## Phase Orchestration

This phase dispatches procedure-depth sub-agents in a three-step
sequence. Best practices research uses the fan-out fan-in pattern:
parallel searches across source types followed by centralized
synthesis.

```
Phase 1: Query Formulation (sequential)
         ↓
Phase 2: Search (parallel)
         ↓
Phase 3: Synthesis (sequential)
```

### Phase 1: Query Formulation

Dispatch `research_procedure` to formulate effective search queries.
Generate 3-5 distinct query variants targeting different aspects.
Include queries for web sources, documentation, and forums.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_practices_phase/reference/query_formulation.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-query-formulation.md`

### Phase 2: Search (Parallel)

Launch all three search sub-agents in parallel in a single message.
Each agent focuses on a different source type.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_practices_phase/reference/web_sources.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_practices_phase/reference/community_sources.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_practices_phase/reference/github_sources.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-web.md`,
  `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-community.md`,
  `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-github.md`

### Phase 3: Synthesis

Dispatch the synthesis procedure. Final phase; produces actionable
recommendations. Resolve contradictions between sources with rationale.
Provide actionable recommendations with supporting evidence.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_best_practices_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-synthesis.md`

## Information Flow

```
Phase 1: Query Formulation
- Receives: {inherited work item context}, TOPIC
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-query-formulation.md

Phase 2: Search
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-web.md,
            {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-community.md,
            {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-github.md

Phase 3: Synthesis
- Receives: {inherited}, Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-synthesis.md
```

Main agent passes report **paths** between phases, never report
content. Phase 3 reads prior phase report paths as INPUT_REPORTs.

## Execution Strategy

1. **Plan**: Define research topic and questions
2. **Query Formulation**: Launch query formulation sub-agent; wait for
   completion
3. **Search**: Launch parallel research sub-agents in single message;
   wait for completion
4. **Synthesis**: Launch synthesis sub-agent with Phase 2 outputs;
   wait for completion
5. **Complete**: Return synthesis summary and report path

## Error Handling

| Situation | Response |
|-----------|----------|
| No sources found for topic | Report and exit |
| Phase produces no findings | Document gap, continue to next phase |
| Synthesis cannot consolidate | Report conflicting findings, exit |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Single source type | Launch agents across web, community, GitHub |
| Sequential search | Launch search agents in parallel |
| Resolving contradictions silently | Document disagreements with rationale |
| Inlining report content | Pass report paths between phases |

## Sub-Layer Launch

When this phase requires procedure-depth execution, launch the
`research_procedure` agent for general step mechanics (query
formulation, source selection, evidence extraction, source evaluation,
bias detection).

Brief template for the `research_procedure` agent — Execute <phase-name> within best practices research:

```text
<ctxmod_activate skill="agentic_workflows_research_procedure" />
Procedure: <procedure-name>
Phase-specific instructions:
  <phase-specific parameters>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch procedures per phase position:

| Phase | Procedure |
|-------|-----------|
| Phase 1 (query formulation) | `reference/query_formulation.md` |
| Phase 2 (web sources) | `reference/source_selection.md` + `reference/evidence_extraction.md` |
| Phase 2 (community sources) | `reference/source_selection.md` + `reference/evidence_extraction.md` |
| Phase 2 (github sources) | `reference/source_selection.md` + `reference/evidence_extraction.md` |
| Phase 3 (synthesis) | `reference/source_evaluation.md` + `reference/bias_detection.md` |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"research current practices" as a sub-step. Launch the
`research_practices_phase` agent from any workflow that needs
industry-practice evidence.

Brief template for the `research_practices_phase` agent — Execute best practices research phase:

```text
<ctxmod_activate skill="agentic_workflows_research_practices_phase" />
Execute the full query → parallel search → synthesis chain.
```

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every research phase
variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_phase_selection.md" />

For a single-scope research that does not need the full three-phase
chain, dispatch the general phase template directly:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_general.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_multi_source.md" />

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_discovery.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped research guidance for the sub-agents this phase launches:

the `research_procedure` skill
