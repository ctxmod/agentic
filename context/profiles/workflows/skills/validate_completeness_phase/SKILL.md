---
description: "Completeness validation phase — criteria discovery, parallel element validation, coverage analysis, report synthesis."
binds_to_depth: phase
---

# Validate Completeness Phase

Completeness validation phase — verify artifacts contain all required
elements and meet coverage expectations. Produces coverage assessments
with gap identification and acceptance-criteria mapping. This is one of
four sibling phase skills in the validation capability (completeness,
consistency, references, structure); they share the same objective
(`validation_objective`) and procedure (`validation_procedure`) layers
and differ only in the reference material they dispatch against.

Completeness validation is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/validate_completeness`
slash-command, or any other domain's workflow agent references this
phase as a "verify completeness" sub-step.

## Work Item Context

This phase benefits from work item context. When Story or Issue context
provided:

- Completeness validates against acceptance criteria
- Reports nest within work item directory
- Missing elements link to work item requirements

When no context provided:

- Completeness validates against schema requirements only
- Reports write to the store's top-level `reports/` (no `--owner`)
- Explicit path argument required

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Required element presence verification needed
- Coverage assessment required
- Acceptance criteria mapping must be performed
- Schema completeness must be verified

Completeness validation confirms required elements exist. Work item
context significantly improves completeness validation by providing
explicit acceptance criteria — strongly recommended for Story and Issue
scopes.

This phase is an explicitly selected dimension, not a routine gate.
Dispatch it when the story's artifacts require completeness assessment —
an explicit request, a workflow step that names it, or a high-risk scope —
never as unconditional routine fan-out. Story completion does not route
through this phase: it routes to the story-completion validator
(Haiku-class), which validates, fixes, rechecks, and commits in one
session and may run an equivalent completeness check internally as part
of its deterministic oracle set without dispatching this phase.

## Phase Orchestration

When explicitly engaged, this phase dispatches objective-depth sub-agents
in a four-step sequential-parallel-sequential sequence. The same objective
(`validation_objective`) runs each time; the phase-specific reference
material below scopes each dispatch.

```
Phase 1: Criteria Discovery (sequential)
         ↓
Phase 2: Element Validation (conditional parallel by criteria type)
         ↓
Phase 3: Coverage Analysis (sequential)
         ↓
Phase 4: Report Synthesis (sequential)
```

### Phase 1: Criteria Discovery

Sequential pattern for single-scope criteria identification. Identify
completeness requirements from work item and schema definitions;
catalog artifacts by type; determine applicable validation criteria
per artifact type.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_scope_discovery.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-criteria-discovery.md`

### Phase 2: Element Validation

Sequential single-agent validation when the engaged scope holds one
criteria type. Parallel dispatch — one validation sub-agent per criteria
type in a single message — only when the explicitly selected scope
contains multiple independent criteria types.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validate_completeness_phase/reference/completeness.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-{sections|frontmatter|acceptance}.md` (parallel)

### Phase 3: Coverage Analysis

Sequential pattern aggregating all element validations. Assess overall
coverage against expectations; calculate coverage metrics by artifact
type; identify distribution gaps (uneven coverage).

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_aggregation.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-coverage-analysis.md`

### Phase 4: Report Synthesis

Sequential pattern producing final validation report. Synthesize
completeness validation findings; provide prioritized remediation
recommendations; include coverage metrics dashboard.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-validation-completeness-final.md`

## Information Flow

```
Phase 1: Criteria Discovery
- Receives: {inherited work item context}, {optional path argument}
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-criteria-discovery.md

Phase 2: Element Validation
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-{sections|frontmatter|acceptance}.md (parallel)

Phase 3: Coverage Analysis
- Receives: {inherited}, Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-coverage-analysis.md

Phase 4: Report Synthesis
- Receives: {inherited}, all previous phase outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-validation-completeness-final.md
```

Main agent passes report **paths** between phases, never report
content. Each phase reads prior phase report paths as INPUT_REPORTs.

## Acceptance Criteria

| Metric | Pass Threshold |
|--------|----------------|
| Required sections missing | 0 |
| Required frontmatter missing | 0 |
| Acceptance criteria unmet | 0 |
| Coverage percentage | >= 90% |

## Variants

**Schema-Only Completeness**: Skip acceptance criteria when no work
item context: `Discovery -> Validation -> Coverage -> Synthesis`.

**Work Item Completeness**: Full workflow with acceptance criteria
validation against work item requirements.

## Error Handling

| Situation | Response |
|-----------|----------|
| No explicit criteria available | Infer from reference exemplars; document inference |
| Scope too broad | Recommend decomposition by artifact type |
| Acceptance criteria ambiguous | Request clarification via work item context |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Validating partial artifacts | Validate complete artifacts only |
| Stopping at first violation | Check all criteria systematically |
| Conflating completeness with correctness | Completeness assesses presence, not quality |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`validation_objective` agent. The objective in turn launches
`validation_procedure` for step mechanics (severity classification,
report creation).

Brief template for the `validation_objective` agent — Execute <phase-name> within completeness validation:

```text
<ctxmod_activate skill="agentic_workflows_validation_objective" />
Variant: completeness
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (discovery) | dispatched via discovery_objective | broad-scan, entry-point-detection |
| Phase 2 (validation) | completeness | severity-classification |
| Phase 3 (coverage) | completeness | severity-classification |
| Phase 4 (synthesis) | completeness | severity-classification |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"verify completeness" as a sub-step. Launch the
`validate_completeness_phase` agent from any workflow that explicitly
selects this dimension for coverage assessment.

Brief template for the `validate_completeness_phase` agent — Execute completeness validation phase:

```text
<ctxmod_activate skill="agentic_workflows_validate_completeness_phase" />
Execute the full discovery -> validation -> coverage -> synthesis chain.
```

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every validation phase
variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_phase_selection.md" />

For a single-scope validation that does not need the full four-phase
chain, dispatch the general phase template directly:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_general.md" />

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_before_execution.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_validation.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped validation guidance for the sub-agents this phase launches:

the `validation_objective` skill
the `validation_procedure` skill
