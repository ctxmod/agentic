---
description: "Refactor implementation phase — reorganize artifacts while preserving behavior through baseline discovery, refactor planning, transformation, and equivalence verification with embedded remediation."
binds_to_depth: phase
---

# Implement Refactor Phase

Refactor implementation phase — reorganize artifacts while preserving
behavior. This is one of three sibling phase skills in the
implementation capability (task, fix, refactor); they share the same
objective (`implementation_objective`) and procedure
(`implementation_procedure`) layers and differ only in the workflow
reference material they dispatch against.

Refactor implementation is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/implement_refactor`
slash-command, the `/implement` router, or any other domain's workflow
agent references this phase as a "implement refactor" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Reorganizing artifacts without changing behavior
- Renaming, moving, extracting, or consolidating components
- Behavior preservation is critical
- Consumer updates must occur atomically with target changes

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a four-step
sequence with a two-part baseline discovery. The same objective
(`implementation_objective`) runs each step; the phase-specific
reference material below scopes each dispatch.

```
Phase 1: Baseline Discovery (two dispatches)
         ├── Structure discovery
         └── Consumer discovery
         ↓
Phase 2: Refactor Planning
         ↓
Phase 3: Transformation ←───────┐
         ↓                      │
Phase 4: Equivalence Verification│
         ├── Structure           │
         ├── References          │
         └── Behavior            │
         ↓                      │
    Issues Found? ─────Yes───────┘
         ↓ No
      Complete
```

### Phase 1: Baseline Discovery

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_baseline.md" />

- Reports: `{REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-baseline-structure.md`,
  `{REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-baseline-consumers.md`

**Workflow-specific notes**:
- Two read-only discovery dispatches: structure baseline, then consumer baseline. This phase agent modifies files in Phase 3 and is **not** granted `allow-parallel-agents: true`; run them one at a time unless your own agent-depth instructions says otherwise.
- Captures comprehensive baseline for comparison
- Documents all consumers requiring updates

### Phase 2: Refactor Planning

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/general.md`

- Report: `{REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-refactor-plan.md`

**Workflow-specific notes**:
- Plans transformation steps in safe sequence
- Ensures behavior preservation strategy
- Plans consumer updates alongside target changes
- Defines rollback strategy

### Phase 3: Transformation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implement_refactor_phase/reference/refactor.md" />

- Report: `{REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-transformation.md`

**Workflow-specific notes**:
- Applies planned transformation only
- Updates all consumers atomically with targets
- Preserves behavior at each step
- Uses Opus model for transformation quality

### Phase 4: Equivalence Verification

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_equivalence.md" />

- Reports: `{REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-verification-{structure,references,behavior}.md`

**Workflow-specific notes**:
- Three read-only verification dispatches: structure, references, behavior (if applicable). Run them one at a time, in the foreground, unless your own agent-depth instructions grants concurrent dispatch.
- Compares transformed state to baseline
- Checks for unplanned changes
- Runs behavioral verification when available

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Phase 1: Baseline Discovery (two dispatches)
- Receives: {inherited}, scope
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-baseline-structure.md
            {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-baseline-consumers.md

Phase 2: Refactor Planning
- Receives: {inherited}, Phase 1 outputs
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-refactor-plan.md

Phase 3: Transformation
- Receives: {inherited}, Phase 2 output
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-transformation.md

Phase 4: Equivalence Verification (three dispatches)
- Receives: {inherited}, transformed artifacts, Phase 1 baselines
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-verification-structure.md
            {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-verification-references.md
            {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-verification-behavior.md (conditional)
```

Main agent passes report **paths** between phases, never report content.

## Execution Strategy

1. **Resolve scope**: Resolve scope and refactor type from arguments
2. **Baseline Discovery**: Launch structure discovery, then consumer discovery
3. **Refactor Planning**: Launch planning sub-agent with baseline outputs
4. **Transformation**: Launch transformation sub-agent with plan
5. **Verification Loop**:
   a. Launch equivalence verification (structure, references, behavior), one at a time
   b. Read verification reports
   c. If issues found: Launch correction, re-verify (max iterations)
   d. If clean: Exit loop
6. **Complete**: Return transformation summary and equivalence status

## Workflow Variants

### Strict Refactor

- Requires behavioral verification
- Zero tolerance for broken references
- Maximum iterations: 5
- Use case: Critical systems

### Pragmatic Refactor

- Behavioral verification when available
- Structural and reference verification mandatory
- Maximum iterations: 3
- Use case: General refactoring

### Incremental Refactor

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

- Large-scope refactoring split into increments
- Verification between increments
- Each increment follows full workflow

## Error Handling

| Situation | Response |
|-----------|----------|
| Baseline discovery fails | Report discovery failure, await decision |
| Transformation fails | Document partial changes, assess rollback, await decision |
| Equivalence verification exceeds iterations | Report persistent issues, await decision |

Error handling follows stop-report-propose-await pattern.

### Transformation Failure

If transformation fails:

1. Report transformation failure with context
2. Document partial changes if any
3. Assess rollback necessity using baseline
4. Propose recovery or alternative approach
5. Await user decision

### Verification Failure (Exceeded Iterations)

If equivalence verification fails after maximum iterations:

1. Report persistent issues with remediation history
2. Compare baseline to current state
3. Assess behavior preservation status
4. Propose manual intervention or rollback
5. Await user decision

## Quality Criteria

| Criterion | Validation Method |
|-----------|-------------------|
| Structure matches plan | Phase 4a compares to plan |
| References intact | Phase 4b verifies all resolve |
| Behavior preserved | Phase 4c runs available verification |
| No unplanned changes | Phase 4a checks scope boundaries |

## Risk Mitigation

### Pre-Transformation Safeguards

- **Baseline documentation**: Comprehensive baseline enables comparison and rollback
- **Scope boundaries**: Explicit include/exclude lists
- **Consumer catalog**: Complete consumer inventory

### During Transformation

- **Atomic updates**: Consumers updated with targets
- **Logging**: Record each transformation step
- **Checkpoints**: Verify after logical units

### Post-Transformation

- **Baseline comparison**: Verify only planned changes occurred
- **Reference verification**: Confirm no broken references
- **Behavior check**: Run verification suites

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping baseline discovery | No comparison reference | Establish baseline first |
| Transforming without planning | Uncontrolled changes | Plan transformation steps |
| Updating targets without consumers | Broken references | Update atomically |
| Skipping equivalence verification | Unconfirmed preservation | Verify equivalence |
| Expanding scope during transformation | Scope creep | Transform planned scope only |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`implementation_objective` agent. The objective in turn launches
`implementation_procedure` for step mechanics.

Brief template for the `implementation_objective` agent — Execute <phase-name> within refactor implementation:

```text
<ctxmod_activate skill="agentic_workflows_implementation_objective" />
Variant: refactor
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"implement refactor" as a sub-step. Launch the
`implement_refactor_phase` agent from any workflow that needs
behavior-preserving reorganization.

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every implementation
phase variant:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_phase_selection.md" />

For a single-scope implementation that does not need the full four-phase
chain, dispatch the general execution template directly:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_general.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_parallel.md" />

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped implementation guidance for the sub-agents this phase launches:

the `implementation_objective` skill
the `implementation_procedure` skill
