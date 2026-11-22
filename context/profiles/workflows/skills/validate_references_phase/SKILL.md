---
description: "References validation phase — reference discovery, parallel resolution validation by reference type, report synthesis."
binds_to_depth: phase
---

# Validate References Phase

References validation phase — verify cross-references resolve
correctly and maintain integrity across artifacts. Produces broken-
link inventories with classification by reference type and severity.
This is one of four sibling phase skills in the validation capability
(completeness, consistency, references, structure); they share the
same objective (`validation_objective`) and procedure
(`validation_procedure`) layers and differ only in the reference
material they dispatch against.

References validation is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/validate_references`
slash-command, or any other domain's workflow agent references this
phase as a "verify references" sub-step.

## Work Item Context

When work item context provided:

- Reports nest within work item directory
- Reference scope may expand to include work item dependencies
- Findings link to work item context

When no context provided:

- Reports write to the store's top-level `reports/` (no `--owner`)
- Explicit path argument required

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Link integrity verification needed
- Cross-reference resolution required
- Compulsory read validation must be performed
- Reference targets must be verified

Reference validation focuses on link integrity. Broken compulsory
reads are always errors — they block sub-agent execution. Broken
optional references are warnings unless they appear in critical paths.

This phase is an explicitly selected dimension, not a routine gate.
Dispatch it when the story's artifacts require reference-integrity
assessment — an explicit request, a workflow step that names it, or a
high-risk scope — never as unconditional routine fan-out. Story
completion does not route through this phase: it routes to the
story-completion validator (Haiku-class), which validates, fixes,
rechecks, and commits in one session and may run an equivalent
reference-resolution check internally as part of its deterministic
oracle set without dispatching this phase.

## Phase Orchestration

When explicitly engaged, this phase dispatches objective-depth sub-agents
in a three-step sequential-parallel-sequential sequence. The same objective
(`validation_objective`) runs each time; the phase-specific reference
material below scopes each dispatch.

```
Phase 1: Reference Discovery (sequential)
         ↓
Phase 2: Resolution Validation (conditional parallel by reference type)
         ↓
Phase 3: Report Synthesis (sequential)
```

### Phase 1: Reference Discovery

Sequential pattern for single-scope reference cataloging. Catalog all
references within scope; classify references by type (sak-tags,
markdown-links, compulsory-reads); document reference base paths for
resolution.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_scope_discovery.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-reference-discovery.md`

### Phase 2: Resolution Validation

Sequential single-agent validation when the engaged scope holds one
reference type. Parallel dispatch — one validation sub-agent per
reference type in a single message — only when the explicitly selected
scope contains multiple independent reference types. Each agent resolves
references against actual targets and documents broken references with
locations and failure reasons.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validate_references_phase/reference/references.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-{sak-refs|md-refs|compulsory-refs}.md` (parallel)

### Phase 3: Report Synthesis

Sequential pattern producing final reference validation report. Group
findings by reference type; classify broken required references as
Errors; classify broken optional references as Warnings.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-validation-references-final.md`

## Information Flow

```
Phase 1: Reference Discovery
- Receives: {inherited work item context}, {optional path argument}
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-reference-discovery.md

Phase 2: Resolution Validation
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-{sak-refs|md-refs|compulsory-refs}.md (parallel)

Phase 3: Report Synthesis
- Receives: {inherited}, all previous phase outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-validation-references-final.md
```

Main agent passes report **paths** between phases, never report
content. Each phase reads prior phase report paths as INPUT_REPORTs.

## Acceptance Criteria

| Metric | Pass Threshold |
|--------|----------------|
| Broken required references | 0 |
| Broken optional references | <= 3 |
| Circular references | 0 |

## Variants

**Tag-Only Reference Validation**: Skip markdown links:
`Discovery -> Tag Validation -> Synthesis`.

**Compulsory-Read Focus**: For agent-template compulsory reads only:
`Discovery -> Compulsory-Read Validation -> Synthesis`.

## Error Handling

| Situation | Response |
|-----------|----------|
| External URL inaccessible | Use cached validation; flag for manual verification |
| Relative path base ambiguous | Document assumption; resolve from source location |
| Circular reference detected | Verify intent (bidirectional links valid); flag infinite loops |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Validating content of referenced files | Reference validation checks resolution, not content |
| Treating optional-reference failures as errors | Classify by required/optional distinction |
| Skipping anchor validation | Anchors without matching sections are broken references |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`validation_objective` agent. The objective in turn launches
`validation_procedure` for step mechanics (severity classification,
report creation).

Brief template for the `validation_objective` agent — Execute <phase-name> within references validation:

```text
<ctxmod_activate skill="agentic_workflows_validation_objective" />
Variant: references
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (discovery) | dispatched via discovery_objective | broad-scan, entry-point-detection |
| Phase 2 (validation) | references | references, severity-classification |
| Phase 3 (synthesis) | references | severity-classification |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"verify references" as a sub-step. Launch the
`validate_references_phase` agent from any workflow that explicitly
selects this dimension for cross-reference integrity assessment.

Brief template for the `validate_references_phase` agent — Execute references validation phase:

```text
<ctxmod_activate skill="agentic_workflows_validate_references_phase" />
Execute the full discovery -> validation -> synthesis chain.
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
