---
description: "Academic research phase — peer-reviewed source discovery, citation analysis, evidence synthesis across scholarly databases."
binds_to_depth: phase
---

# Research Academic Phase

Academic research phase — research foundational concepts and
theoretical foundations from academic papers, formal specifications,
and scholarly sources. This is one of three sibling phase skills in the
research capability (academic, compare, practices); they share the
general procedure layer (`research_procedure`) and differ in scope and
reference material. The academic phase additionally dispatches a paired
academic procedure skill (`scholarly_research_procedure`) for
academic-specific step mechanics.

Academic research is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/scholarly_research`
slash-command, or any other domain's workflow agent references this
phase as a "research from academic sources" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/research_briefs.md" />

## When to Use

Use this phase when:

- Researching theoretical foundations and formal concepts
- Gathering evidence from academic literature
- Understanding formal specifications and standards
- Connecting theory to practical applications
- Requiring rigorous evidence quality assessment

## Phase Orchestration

This phase dispatches procedure-depth sub-agents in a three-step
sequence. Academic research uses Opus for synthesis due to the
complexity of integrating formal and theoretical sources. Query
formulation and parallel search phases use Sonnet.

```
Phase 1: Query Formulation (sequential)
         ↓
Phase 2: Academic Search (parallel)
         ↓
Phase 3: Synthesis (sequential, Opus)
```

### Phase 1: Query Formulation

Dispatch `scholarly_research_procedure` to formulate academic search
queries. Generate 3-5 distinct query variants for academic databases;
include formal terminology and research-area keywords.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_phase/reference/query_formulation.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-query-formulation.md`

### Phase 2: Academic Search (Parallel)

Launch all three academic search sub-agents in parallel in a single
message. Each agent focuses on a different academic source type.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_phase/reference/academic_papers.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_phase/reference/specifications.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_phase/reference/surveys.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-papers.md`,
  `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-specifications.md`,
  `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-surveys.md`

### Phase 3: Synthesis

Dispatch the synthesis procedure; produces comprehensive synthesis.
Uses Opus for complex academic synthesis. Connect theoretical
foundations to practical applications.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_academic_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-synthesis.md`

## Information Flow

```
Phase 1: Query Formulation
- Receives: {inherited work item context}, TOPIC
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-query-formulation.md

Phase 2: Academic Search
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-papers.md,
            {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-specifications.md,
            {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-research-surveys.md

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
3. **Academic Search**: Launch parallel research sub-agents in single
   message; wait for completion
4. **Synthesis**: Launch Opus synthesis sub-agent with Phase 2 outputs;
   wait for completion
5. **Complete**: Return synthesis summary and report path

## Error Handling

| Situation | Response |
|-----------|----------|
| No academic sources found for topic | Report and exit |
| Phase produces no findings | Document gap, continue to next phase |
| Synthesis cannot consolidate | Report conflicting findings, exit |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Grey literature as primary evidence | Prioritize peer-reviewed sources |
| Sequential academic search | Launch academic agents in parallel |
| Synthesizing before extraction completes | Complete extraction then synthesize |
| Inlining report content | Pass report paths between phases |

## Sub-Layer Launch

When this phase requires procedure-depth execution, launch the
`scholarly_research_procedure` agent for academic-specific step
mechanics (academic query formulation, academic source selection,
academic evidence extraction, academic source evaluation, academic
bias detection, broad scan, academic pipeline).

Brief template for the `scholarly_research_procedure` agent — Execute <phase-name> within academic research:

```text
<ctxmod_activate skill="agentic_academic_scholarly_research_procedure" />
Procedure: <procedure-name>
Phase-specific instructions:
  <phase-specific parameters>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch procedures per phase position:

| Phase | Procedure |
|-------|-----------|
| Phase 1 (query formulation) | `reference/query_formulation_academic.md` |
| Phase 2 (papers) | `reference/source_selection_academic.md` + `reference/evidence_extraction_academic.md` |
| Phase 2 (specifications) | `reference/source_selection_academic.md` + `reference/evidence_extraction_academic.md` |
| Phase 2 (surveys) | `reference/source_selection_academic.md` + `reference/evidence_extraction_academic.md` |
| Phase 3 (synthesis) | `reference/source_evaluation_academic.md` + `reference/bias_detection_academic.md` |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"research from academic sources" as a sub-step. Launch the
`scholarly_research_phase` agent from any workflow that needs
theoretical grounding or formal validation.

Brief template for the `scholarly_research_phase` agent — Execute academic research phase:

```text
<ctxmod_activate skill="agentic_academic_scholarly_research_phase" />
Execute the full query → parallel search → synthesis chain.
```

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every research phase
variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_phase_selection.md" />

For a single-scope academic research that does not need the full
three-phase chain, dispatch the full academic pipeline directly:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_phase/reference/academic.md" />

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_discovery.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped research guidance for the sub-agents this phase launches:

the `scholarly_research_procedure` skill
the `research_procedure` skill
