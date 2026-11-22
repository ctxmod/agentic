---
description: "Comprehensive multi-dimensional analysis phase — scope resolution, parallel discovery, parallel assessment, aggregation, Opus synthesis."
binds_to_depth: phase
---

# Analyze Comprehensive Phase

Comprehensive analysis phase — orchestrate analysis across multiple
dimensions (gap assessment, alternative comparison, quality
evaluation, technical debt inventory) for major decisions requiring
multi-dimensional assessment. This is one of five sibling phase skills
in the analysis capability (compare, debt, gap, quality,
comprehensive); they share the same objective (`analysis_objective`)
and procedure (`analysis_procedure`) layers and differ only in the
reference material they dispatch against.

Comprehensive analysis is a **phase, not a workflow**. It has no
matching slash-command — it is model-invoked when the main agent
determines that a decision requires multi-dimensional assessment
beyond a single focused phase. Other domains' workflow agents may
also reference this phase as an "analyze comprehensively" sub-step.

## Invocation

This phase carries no absorbed slash-command. The main agent
dispatches it directly when:

- Major decisions require multi-dimensional assessment
- Incomplete analysis carries high risk
- Strategic planning needs comprehensive evidence
- Architectural changes demand thorough evaluation

Reports nest within the work item directory if work-item context is
provided (EPIC_PATH, STORY_PATH, ISSUE_PATH, TASK_PATH). Otherwise,
reports write to the store's top-level `reports/` (no `--owner`).

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when a decision spans multiple analysis dimensions and
no single focused phase (compare, debt, gap, quality) is sufficient.
Comprehensive analysis runs multiple parallel discovery and
assessment agents. Use for decisions where systematic coverage
matters more than efficiency.

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a five-step
fan-out fan-in sequence with extensive parallelization. The same
objective (`analysis_objective`) runs each time; the phase-specific
reference material below scopes each dispatch.

```
Phase 1: Scope Resolution
         ↓
Phase 2: Parallel Discovery (Fan-Out)
         ├── Current state
         ├── Target state
         ├── Alternatives
         ├── Quality indicators
         └── Debt indicators
         ↓
Phase 3: Parallel Assessment (Fan-Out)
         ├── Gap assessment
         ├── Comparison assessment
         ├── Quality assessment
         └── Debt assessment
         ↓
Phase 4: Aggregation (Fan-In)
         ↓
Phase 5: Synthesis (Opus)
```

### Phase 1: Scope Resolution

Sequential pattern for single-scope resolution. Determine applicable
analysis dimensions; catalog artifacts and identify dimension
applicability; determine target state sources and alternatives.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_phase_selection.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-scope-resolution.md`

### Phase 2: Parallel Discovery (Fan-Out)

Parallel pattern: launch discovery agents for all applicable
dimensions. Each agent works on independent scope; fan-in at Phase 4
aggregation.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_phase_selection.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-discovery-{dimension}.md` (parallel)

### Phase 3: Parallel Assessment (Fan-Out)

Parallel pattern: launch assessment agents for all applicable
dimensions. Each agent consumes relevant discovery reports; fan-in at
Phase 4 aggregation.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_phase_selection.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-assessment-{dimension}.md` (parallel)

### Phase 4: Aggregation (Fan-In)

Fan-in pattern aggregating all assessments. Group findings by theme,
not by analysis type; identify cross-dimension correlations; flag
contradictions with source attributions.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analyze_comprehensive_phase/reference/aggregation.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-aggregation.md`

### Phase 5: Synthesis (Opus)

Sequential pattern with Opus for complex multi-dimensional synthesis.
Provide executive summary with key findings per dimension; structure
recommendations by priority; include comparison matrix if
alternatives assessed; document trade-offs and context-dependent
guidance.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-05-synthesis.md`

## Information Flow

```
Phase 1: Scope Resolution
- Receives: {inherited work item context}
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-scope-resolution.md

Phase 2: Parallel Discovery (Fan-Out)
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-discovery-{dimension}.md (parallel)

Phase 3: Parallel Assessment (Fan-Out)
- Receives: {inherited}, Phase 1 + Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-assessment-{dimension}.md (parallel)

Phase 4: Aggregation (Fan-In)
- Receives: {inherited}, Phase 3 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-aggregation.md

Phase 5: Synthesis
- Receives: {inherited}, all previous phase outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-05-synthesis.md
```

Main agent passes report **paths** between phases, never report
content. Each phase reads prior phase report paths as INPUT_REPORTs.

## Variants

**Targeted Comprehensive Analysis**: When a subset of dimensions is
required, skip inapplicable dimensions based on scope resolution.

**Research-Heavy Analysis**: When external evidence informs analysis,
add a research phase between discovery and assessment.

**Iterative Analysis**: For staged comprehensive assessment, iterate
`Discovery → Assessment → Aggregation` per stage until complete, then
synthesize.

## Error Handling

| Situation | Response |
|-----------|----------|
| Scope too broad for single session | Decompose into focused analyses |
| Discovery agents produce contradictions | Flag in aggregation, address in synthesis |
| Assessment incomplete for dimension | Proceed with available data, note limitations |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Running all dimensions for simple decisions | Use focused analysis phases instead |
| Skipping aggregation before synthesis | Aggregation reduces synthesis complexity |
| Using Sonnet for multi-dimensional synthesis | Use Opus for complex synthesis |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`analysis_objective` agent. The objective in turn launches
`analysis_procedure` for step mechanics.

Brief template for the `analysis_objective` agent — Execute <phase-name> within comprehensive analysis:

```text
<ctxmod_activate skill="analysis_objective" />
Variant: synthesis
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (scope) | dispatched via discovery_objective | broad-scan, entry-point-detection |
| Phase 2 (discovery) | dispatched via discovery_objective | broad-scan, pattern-detection, boundary-validation |
| Phase 3 (assessment) | comparative / gap / synthesis | criteria-weighting, impact-scoring, contradiction-resolution |
| Phase 4 (aggregation) | synthesis | pattern-aggregation, contradiction-resolution |
| Phase 5 (synthesis) | synthesis | pattern-aggregation, confidence-assessment |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"analyze comprehensively" as a sub-step. Launch the
`analyze_comprehensive_phase` agent from any workflow that needs
multi-dimensional analysis coverage.

Brief template for the `analyze_comprehensive_phase` agent — Execute comprehensive analysis phase:

```text
<ctxmod_activate skill="analyze_comprehensive_phase" />
Execute the full scope → discovery → assessment → aggregation → synthesis chain.
```

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every analysis phase
variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_phase_selection.md" />

For a single-scope analysis that does not need the full five-phase
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
