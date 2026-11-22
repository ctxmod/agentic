---
description: "Structure validation phase — criteria resolution, parallel structural validation per artifact type, report synthesis."
binds_to_depth: phase
---

# Validate Structure Phase

Structure validation phase — verify artifacts comply with defined
schemas and structural conventions. Produces schema-compliance
assessments with missing-element and ordering-violation inventories.
This is one of four sibling phase skills in the validation capability
(completeness, consistency, references, structure); they share the
same objective (`validation_objective`) and procedure
(`validation_procedure`) layers and differ only in the reference
material they dispatch against.

Structure validation is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/validate_structure`
slash-command, or any other domain's workflow agent references this
phase as a "verify structure" sub-step.

## Work Item Context

When work item context provided:

- Reports nest within work item directory
- Scope may derive from work item boundaries
- Findings link to relevant work items

When no context provided:

- Reports write to the store's top-level `reports/` (no `--owner`)
- Explicit path argument required

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Schema compliance verification needed
- Format verification required
- Structural standards must be validated
- Content quality assessment not required

Structural validation focuses on format and organization. For
reference integrity, use the references phase. For cross-artifact
alignment, use the consistency phase. For required element presence,
use the completeness phase.

This phase is an explicitly selected dimension, not a routine gate.
Dispatch it when the story's artifacts require structural assessment —
an explicit request, a workflow step that names it, or a high-risk
scope — never as unconditional routine fan-out. Story completion does
not route through this phase: it routes to the story-completion
validator (Haiku-class), which validates, fixes, rechecks, and commits
in one session and may run an equivalent structural check internally as
part of its deterministic oracle set without dispatching this phase.

## Phase Orchestration

When explicitly engaged, this phase dispatches objective-depth sub-agents
in a three-step sequential-parallel-sequential sequence. The same objective
(`validation_objective`) runs each time; the phase-specific reference
material below scopes each dispatch.

```
Phase 1: Criteria Resolution (sequential)
         ↓
Phase 2: Structural Validation (parallel per artifact type if needed)
         ↓
Phase 3: Report Synthesis (sequential)
```

### Phase 1: Criteria Resolution

Sequential pattern for single-scope criteria identification. Identify
structural validation criteria; locate governing schemas per artifact
type; document inferred requirements when explicit schemas absent.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_scope_discovery.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-criteria-resolution.md`

### Phase 2: Structural Validation

Parallel pattern when scope contains multiple independent artifact
types; otherwise sequential single-agent validation. Check required
elements present and correctly positioned; check prohibited elements
absent; verify nesting and ordering constraints.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validate_structure_phase/reference/structural.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

- Report(s): `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-structure.md` (or `02-val-structure-{type}.md` parallel)

### Phase 3: Report Synthesis

Sequential pattern producing final structural validation report.
Group findings by violation type (missing, mispositioned, malformed);
note schema drift where applicable; recommend schema-definition
remediation where drift detected.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-validation-structure-final.md`

## Information Flow

```
Phase 1: Criteria Resolution
- Receives: {inherited work item context}, {optional path argument}
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-criteria-resolution.md

Phase 2: Structural Validation
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-structure.md (or parallel)

Phase 3: Report Synthesis
- Receives: {inherited}, all previous phase outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-validation-structure-final.md
```

Main agent passes report **paths** between phases, never report
content. Each phase reads prior phase report paths as INPUT_REPORTs.

## Acceptance Criteria

| Metric | Pass Threshold |
|--------|----------------|
| Errors | 0 |
| Warnings | <= 5 |
| Coverage | All artifacts in scope validated |

## Variants

**Schema-Only Structure Validation**: Skip convention checks:
`Resolution -> Schema Validation -> Synthesis`.

**Multi-Artifact Structure Validation**: Parallel validation when
scope contains independent artifact types.

## Error Handling

| Situation | Response |
|-----------|----------|
| No explicit schema available | Infer from reference exemplars; document inference |
| Schema and practice diverge | Validate against authoritative definition; flag drift |
| Partial structure definition | Validate documented requirements; note undocumented sections |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Stopping at first violation | Check all structural requirements systematically |
| Treating convention violations as structural errors | Distinguish schema compliance from convention adherence |
| Validating partial artifacts | Validate complete artifacts only |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`validation_objective` agent. The objective in turn launches
`validation_procedure` for step mechanics (severity classification,
report creation).

Brief template for the `validation_objective` agent — Execute <phase-name> within structure validation:

```text
<ctxmod_activate skill="agentic_workflows_validation_objective" />
Variant: structural
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (resolution) | dispatched via discovery_objective | broad-scan, entry-point-detection |
| Phase 2 (validation) | structural | structural, severity-classification |
| Phase 3 (synthesis) | structural | severity-classification |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"verify structure" as a sub-step. Launch the
`validate_structure_phase` agent from any workflow that explicitly
selects this dimension for schema-compliance assessment.

Brief template for the `validate_structure_phase` agent — Execute structure validation phase:

```text
<ctxmod_activate skill="agentic_workflows_validate_structure_phase" />
Execute the full resolution -> validation -> synthesis chain.
```

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every validation phase
variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_phase_selection.md" />

For a single-scope validation that does not need the full three-phase
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
