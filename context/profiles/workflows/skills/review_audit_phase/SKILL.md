---
description: "Comprehensive quality audit through multi-criteria parallel validation and findings synthesis."
binds_to_depth: phase
---

# Review Audit Phase

Comprehensive quality audit phase — orchestrate parallel validation
across structural, standards, completeness, and reference criteria,
then synthesize findings into a prioritized remediation plan. This
phase combines the review workflow (scope discovery → multi-criteria
validation → synthesis → remediation) with audit-specific criteria.

Review audit is a **phase, not a workflow**. The main agent invokes
it directly via the `/review_audit` slash-command.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_review.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

## When to Use

Use this phase when:

- Artifacts require comprehensive quality assessment
- Multi-criteria validation improves coverage
- Synthesis of findings into prioritized remediation is expected
- Parallel evaluation improves efficiency

- the /review command selection guidance — when the full /review gate
  is warranted versus this specialized variant — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/review.md" />

## Phase Orchestration

This phase dispatches workflow-depth sub-agents across four phases.
The review workflow reference carries the per-phase shape; the
synthesis applies pattern aggregation and severity consolidation.

```
Phase 1: Scope Discovery (Sonnet)
         ↓
Phase 2: Multi-Criteria Validation (Sonnet, parallel)
         ├── Structural validation
         ├── Standards validation
         ├── Completeness validation
         └── Reference validation
         ↓
Phase 3: Findings Synthesis (Sonnet)
         ↓
Phase 4: Remediation Planning (Sonnet)
         ↓
      Complete
```

### Phase 1: Scope Discovery (Sonnet)

Launch the `workflow` agent for scope discovery:

Brief template for a workflow-depth agent — Audit scope discovery:

```text
Execute the scope-discovery phase of the review audit workflow.
Scope: <user-provided scope or current context>
```

**Command-specific notes**:
- Identify artifacts within audit scope
- Establish evaluation context and governing criteria
- When the scope is VCS-based ("since X"), pin the diff base before any
  Phase 2 dispatch: capture the changeset diff command once in
  merge-base form (`git diff <fp>...HEAD`) plus the commit list; verify
  the base resolves (`git rev-parse <fp>`) and the diff is non-empty —
  fail loud in discovery, never inside parallel validators
- Produce artifact inventory with validation requirements

### Phase 2: Multi-Criteria Validation (Sonnet, parallel)

Launch four parallel validation agents (one per criterion):

Brief template for a workflow-depth agent — Structural validation:

```text
Execute the structural validation phase.
Criterion: structural
```

**Command-specific notes**:
- All validators assess full artifacts within scope
- Phase 1 discovery report provides artifact paths and criteria

### Phase 3: Findings Synthesis (Sonnet)

Launch the synthesis agent:

Brief template for a workflow-depth agent — Audit findings synthesis:

```text
Execute the synthesis phase (fan-in of all Phase 2 validation reports).
```

**Command-specific notes**:
- Fan-in aggregation of all Phase 2 validation reports
- When a Spec Fidelity criterion is active, aggregate per axis under
  per-axis headings; never merge or rerank findings across axes
- End with a one-line per-axis summary carrying the worst finding within
  each axis; no single cross-axis winner
- Identify cross-validation patterns
- Consolidate severity across criteria
- Resolve conflicts between validators

The axes stay separate because work can pass one and fail the other —
convention-conformant artifacts implementing the wrong thing, or
faithful artifacts breaking the repo's conventions; separate reporting
stops one axis from masking the other.

### Phase 4: Remediation Planning (Sonnet)

Launch the planning agent:

Brief template for a phase-depth agent — Audit remediation planning:

```text
Execute the remediation planning phase (prioritize corrections by impact).
```

**Command-specific notes**:
- Plan corrections based on synthesis findings
- Prioritize by severity and impact
- Group related corrections for efficient remediation
- May propose work items for tracking major remediation efforts

## Information Flow

```
Phase 1: Discovery
- Receives: scope argument
- Produces: artifact inventory + criteria

Phase 2: Validation (parallel)
- Receives: Phase 1 inventory
- Produces: per-criterion validation reports

Phase 3: Synthesis
- Receives: all Phase 2 reports
- Produces: consolidated findings

Phase 4: Remediation
- Receives: Phase 3 synthesis
- Produces: prioritized remediation plan
```

Main agent passes report **paths** between phases, never report content.

## Validation Criteria

Default audit validates these criteria:

| Criterion | Focus |
|-----------|-------|
| Structural | Format, organization, schema compliance |
| Standards | Convention adherence, style compliance |
| Completeness | Required element coverage |
| References | Link integrity, dependency resolution |

Main agents may add criteria based on audit purpose (compliance, readiness, quality gate).

Name **Spec Fidelity** when the audit has an originating work item — a
first-class addable criterion with the same three finding classes as the
PR phase: (a) requirements the work item asked for that are missing or
partial; (b) behavior in scope that was not asked for (scope creep);
(c) requirements that look implemented but wrongly. Resolve the
work-item source in this order: work-item references in commit messages
→ a user-supplied path → a file matching the branch or feature name →
ask the user; if nothing resolves, skip the axis and report "no work
item available".

The Standards criterion carries the judgement-call smell baseline
beneath documented standards: a documented repo standard overrides the
baseline, each smell is a labelled judgement call rather than a hard
violation, and anything tooling enforces is skipped — the
semantic-choices-versus-machine-invariants split. The full 12-smell
catalog: <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::review/reference/review_smell_baseline.md" />.

## Error Handling

| Situation | Response |
|-----------|----------|
| Scope yields zero artifacts | Report and exit (nothing to audit) |
| Validation reports conflict | Synthesis resolves via severity consolidation |
| Remediation exceeds scope | Flag for separate work item |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Serializing Phase 2 validators | Run all four in parallel |
| Inlining validation findings into orchestrator | Pass report paths between phases |
| Skipping remediation planning | Always produce a prioritized plan |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`validation_objective` or `analysis_objective` agents per phase.

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"audit quality" as a sub-step. Launch the `review_audit_phase` agent
from any workflow that needs comprehensive quality assessment.

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile
