---
description: "Findings (fix) implementation phase — apply corrections identified by validation reports through findings resolution, correction planning, correction implementation, and re-validation with embedded remediation."
binds_to_depth: phase
---

# Implement Fix Phase

Findings (fix) implementation phase — apply corrections identified by
validation reports. This is one of three sibling phase skills in the
implementation capability (task, fix, refactor); they share the same
objective (`implementation_objective`) and procedure
(`implementation_procedure`) layers and differ only in the workflow
reference material they dispatch against.

Fix implementation is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/implement_fix` slash-command,
the `/implement` router, or any other domain's workflow agent
references this phase as a "implement fix" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Validation reports identify issues requiring correction
- Scope is defined by validation findings rather than task requirements
- Corrections must be applied systematically with verification

## Phase Orchestration

This phase dispatches objective-depth sub-agents in a four-step
sequence. The same objective (`implementation_objective`) runs each
step; the phase-specific reference material below scopes each dispatch.

```
Phase 1: Findings Resolution
         ↓
Phase 2: Correction Planning
         ↓
Phase 3: Correction Implementation ←──┐
         ↓                            │
Phase 4: Re-Validation                │
         ↓                            │
    Issues Found? ─────Yes────────────┘
         ↓ No
      Complete
```

### Phase 1: Findings Resolution

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_findings_resolution.md" />

- Report: `{REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-findings-resolution.md`

**Workflow-specific notes**:
- Loads validation report specified by `--input` or most recent validation report
- Filters findings by severity threshold (`--severity` parameter)
- Groups findings by affected artifact for systematic planning

### Phase 2: Correction Planning

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/correction.md`

- Report: `{REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-correction-plan.md`

**Workflow-specific notes**:
- Plans minimal corrections addressing each finding
- Sequences corrections to avoid conflicts
- Does not expand scope beyond identified findings

### Phase 3: Correction Implementation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implement_fix_phase/reference/findings.md" />

- Report: `{REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-correction-implementation.md`

**Workflow-specific notes**:
- Applies corrections according to plan
- Uses Opus model for modification quality
- Documents each correction with finding reference

### Phase 4: Re-Validation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_general.md" />
`ctx:github.com/ctxmod/agentic/context::shared::remediation_phase@workflows/reference/targeted.md`

- Report: `{REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-revalidation.md`

**Workflow-specific notes**:
- Validates using same criteria as original validation
- Checks for regression or new issues introduced
- Validation loop (max 3 iterations) until findings resolved

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Phase 1: Findings Resolution
- Receives: {inherited}, validation report path
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-findings-resolution.md

Phase 2: Correction Planning
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-correction-plan.md

Phase 3: Correction Implementation
- Receives: {inherited}, Phase 2 output
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-correction-implementation.md

Phase 4: Re-Validation
- Receives: {inherited}, modified artifacts, original validation criteria
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{nn}-revalidation.md
```

Main agent passes report **paths** between phases, never report content.

## Execution Strategy

1. **Resolve input**: Resolve validation report (explicit parameter or most recent)
2. **Findings Resolution**: Launch findings resolution sub-agent; wait
3. **Correction Planning**: Launch planning sub-agent with findings output
4. **Correction Implementation**: Launch implementation sub-agent with plan
5. **Verification Loop**:
   a. Launch re-validation sub-agent targeting modified artifacts
   b. Read verification report
   c. If issues found: Launch further correction, re-validate (max 3)
   d. If clean: Exit loop
6. **Complete**: Return correction summary and re-validation status

## Workflow Variants

### Strict Findings Implementation

- Terminates only on zero findings
- No severity threshold adjustment
- Maximum iterations: 3
- Use case: Compliance requirements

### Pragmatic Findings Implementation

- Filters by severity threshold
- Accepts warnings below threshold
- Maximum iterations: 3
- Use case: General quality improvement

## Error Handling

| Situation | Response |
|-----------|----------|
| Validation report cannot be resolved | Report resolution failure, await decision |
| Correction implementation fails | Document partial changes, await decision |
| Re-validation exceeds iterations | Report persistent findings, await decision |

Error handling follows stop-report-propose-await pattern.

### Unresolved Findings (Exceeded Iterations)

If validation loop exceeds maximum iterations:

1. Report persistent findings with remediation history
2. Document what remains unresolved
3. Assess partial success value
4. Propose manual intervention or scope reduction
5. Await user decision

## Quality Criteria

- No error-severity violations introduced
- All findings addressed (per severity threshold)
- No new violations introduced
- References remain intact

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Expanding scope beyond findings | Scope creep, iteration exhaustion | Fix only identified findings |
| Applying corrections without planning | Conflicts, incomplete fixes | Plan corrections first |
| Skipping re-validation | Unconfirmed outcomes | Always verify after correction |
| Fixing warnings before errors | Errors block acceptance | Prioritize by severity |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`implementation_objective` agent. The objective in turn launches
`implementation_procedure` for step mechanics.

Brief template for the `implementation_objective` agent — Execute <phase-name> within fix implementation:

```text
<ctxmod_activate skill="implementation_objective" />
Variant: fix
Phase-specific instructions:
  <phase-specific parameters>
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"implement fix" as a sub-step. Launch the `implement_fix_phase` agent
from any workflow that needs findings-driven correction.

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
