---
description: "Consistency validation phase — pattern discovery, parallel consistency validation, cross-reference alignment, report synthesis."
binds_to_depth: phase
---

# Validate Consistency Phase

Consistency validation phase — verify artifacts maintain consistent
terminology, patterns, and conventions across related files. Produces
alignment assessments with terminology drift detection and
cross-reference accuracy verification. This is one of four sibling
phase skills in the validation capability (completeness, consistency,
references, structure); they share the same objective
(`validation_objective`) and procedure (`validation_procedure`) layers
and differ only in the reference material they dispatch against.

Consistency validation is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/validate_consistency`
slash-command, or any other domain's workflow agent references this
phase as a "verify consistency" sub-step.

## Work Item Context

When work item context provided:

- Reports nest within work item directory
- Consistency scope may derive from work item boundaries
- Findings link to work item acceptance criteria

When no context provided:

- Reports write to the store's top-level `reports/` (no `--owner`)
- Explicit path argument required

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Cross-artifact alignment verification needed
- Terminology uniformity assessment required
- Naming convention validation must be performed
- Formatting consistency must be verified

Consistency validation assesses uniformity across artifacts.
Consistency issues often indicate documentation that evolved without
coordinated updates — consider establishing conventions to prevent
recurrence.

This phase is an explicitly selected dimension, not a routine gate.
Dispatch it when the story's artifacts require consistency assessment —
an explicit request, a workflow step that names it, or a high-risk scope —
never as unconditional routine fan-out. Story completion does not route
through this phase: it routes to the story-completion validator
(Haiku-class), which validates, fixes, rechecks, and commits in one
session and may run an equivalent consistency check internally as part
of its deterministic oracle set without dispatching this phase.

## Phase Orchestration

When explicitly engaged, this phase dispatches objective-depth sub-agents
in a four-step sequential-parallel-sequential sequence. The same objective
(`validation_objective`) runs each time; the phase-specific reference
material below scopes each dispatch.

```
Phase 1: Pattern Discovery (sequential)
         ↓
Phase 2: Consistency Validation (conditional parallel by aspect)
         ↓
Phase 3: Cross-Reference Alignment (sequential)
         ↓
Phase 4: Report Synthesis (sequential)
```

### Phase 1: Pattern Discovery

Sequential pattern for single-scope pattern discovery. Discover
patterns and conventions to validate; catalog artifacts and identify
consistency dimensions; document baseline terminology and conventions.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_scope_discovery.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-pattern-discovery.md`

### Phase 2: Consistency Validation

Sequential single-agent validation when the engaged scope holds one
aspect. Parallel dispatch — one validation sub-agent per consistency
aspect (terminology, formatting, naming) in a single message — only when
the explicitly selected scope contains multiple independent aspects.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validate_consistency_phase/reference/consistency.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-{terminology|formatting|naming}.md` (parallel)

### Phase 3: Cross-Reference Alignment

Sequential pattern verifying reference alignment. Verify references
describe targets accurately; check term spelling consistency; identify
conflicting definitions.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validate_consistency_phase/reference/consistency.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-val-alignment.md`

### Phase 4: Report Synthesis

Sequential pattern producing final consistency report. Group findings
by conflict type; note terminology drift patterns; recommend convention
establishment.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_synthesis.md" />

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-validation-consistency-final.md`

## Information Flow

```
Phase 1: Pattern Discovery
- Receives: {inherited work item context}, {optional path argument}
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-pattern-discovery.md

Phase 2: Consistency Validation
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-{terminology|formatting|naming}.md (parallel)

Phase 3: Cross-Reference Alignment
- Receives: {inherited}, Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-val-alignment.md

Phase 4: Report Synthesis
- Receives: {inherited}, all previous phase outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-validation-consistency-final.md
```

Main agent passes report **paths** between phases, never report
content. Each phase reads prior phase report paths as INPUT_REPORTs.

## Acceptance Criteria

| Metric | Pass Threshold |
|--------|----------------|
| Terminology conflicts | 0 |
| Naming inconsistencies | <= 3 |
| Formatting variations | <= 5 |
| Stale cross-references | 0 |

## Variants

**Terminology-Only Consistency**: Skip formatting and naming:
`Pattern Discovery -> Terminology Validation -> Alignment -> Synthesis`.

**Formatting-Focused Consistency**: For style-guide compliance:
`Discovery -> Formatting Validation -> Synthesis`.

## Error Handling

| Situation | Response |
|-----------|----------|
| No baseline established | Derive baseline from most authoritative artifact |
| Intentional variation flagged | Verify intent before flagging as inconsistency |
| Scope crosses version boundaries | Establish version scope before validation |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Conflating intentional variation with inconsistency | Verify intent before flagging |
| Comparing assertions across incompatible scopes | Preserve context in comparison |
| Treating semantic equivalence as inconsistency | Normalize meaning before comparison |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`validation_objective` agent. The objective in turn launches
`validation_procedure` for step mechanics (severity classification,
report creation).

Brief template for the `validation_objective` agent — Execute <phase-name> within consistency validation:

```text
<ctxmod_activate skill="agentic_workflows_validation_objective" />
Variant: consistency
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Objective variant | Procedures launched |
|-------|-------------------|---------------------|
| Phase 1 (discovery) | dispatched via discovery_objective | broad-scan, pattern-detection |
| Phase 2 (validation) | consistency | severity-classification |
| Phase 3 (alignment) | consistency | consistency, severity-classification |
| Phase 4 (synthesis) | consistency | severity-classification |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"verify consistency" as a sub-step. Launch the
`validate_consistency_phase` agent from any workflow that explicitly
selects this dimension for cross-artifact alignment assessment.

Brief template for the `validate_consistency_phase` agent — Execute consistency validation phase:

```text
<ctxmod_activate skill="agentic_workflows_validate_consistency_phase" />
Execute the full discovery -> validation -> alignment -> synthesis chain.
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
