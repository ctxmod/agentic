---
description: "Correct issues identified during validation, addressing findings systematically through variants selected by complexity and scope."
binds_to_depth: phase
---

# Remediation Phase

Correct issues identified during validation. Remediation addresses
findings systematically to bring artifacts into compliance with
acceptance criteria.

Remediation is a **phase-rooted partial domain**: there is no parent
`remediation-workflow` skill (decision 2 — sparse cell omitted). The
phase skill stands on its own; remediation contexts are dispatched
directly into this phase rather than reached via a workflow.

## When to Use

Use remediation phase when:

- Validation identified issues requiring correction
- Artifacts do not meet acceptance criteria
- Systematic correction is more efficient than manual intervention
- Quality convergence through iteration is acceptable

Remediation follows validation and precedes re-validation. It forms
the correction half of validation-remediation loops.

## Variant Selection

Read the variant file matching the assigned task. Do not read all
three — each is independently applicable and reading extras wastes
context.

| Remediation scenario | Read |
|----------------------|------|
| Broad set of validation findings, errors prioritized over warnings | `reference/general.md` |
| Specific findings identified by ID, minimal-scope correction | `reference/targeted.md` |
| Findings require deep reasoning about artifact structure or have non-obvious implications | `reference/complex.md` |

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::remediation_phase/reference/complex.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::remediation_phase/reference/general.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::remediation_phase/reference/targeted.md" />

## Model Selection

**Primary**: Sonnet for straightforward corrections

Sonnet handles clear corrections efficiently.

**Exception**: Use Opus for complex corrections requiring deep
understanding of artifact structure or when corrections have
non-obvious implications. The `complex.md` variant declares this
override explicitly.

## Coordination Pattern

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />

Remediation is typically part of a validation loop. The pattern
iterates validation and remediation until acceptance criteria are met.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />

Within remediation, corrections often proceed sequentially to avoid
introducing conflicts. For independent corrections across
non-overlapping artifacts, parallel remediation is possible.

## Context Requirements

**Parameters provided by main agent**:

| Parameter | Required | Purpose |
|-----------|----------|---------|
| Report (reserved) | Yes | Sub-agent creates the report record just-in-time with `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing |
| INPUT_REPORT | Yes | Validation report with findings |
| ARTIFACT_PATHS | Yes | What to remediate |
| FINDING_IDS | For targeted | Specific findings to address |
| Work item context | If applicable | EPIC_PATH, STORY_PATH, ISSUE_PATH |

## Iteration Limits

Default: 3 iterations.

Rationale:

- Most issues resolve in 1-2 iterations
- 3 iterations catches stubborn issues
- More than 3 suggests fundamental problems

When limit is reached:

1. Document remaining issues
2. Report partial success
3. Propose manual intervention
4. Await user decision

## Transition Criteria

Remediation completes and transitions to re-validation when:

- Targeted findings have been addressed
- Corrections are documented
- Deferred items are justified
- Artifacts are ready for re-validation

If remediation introduces new issues, document and proceed to
re-validation for detection.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Remediation without validation | No guidance for corrections | Validate first |
| Fixing warnings before errors | Errors block acceptance | Prioritize by severity |
| Ignoring correction failures | Incomplete remediation | Document and escalate |
| Over-correction | Introducing new issues | Apply minimal fixes |
| Unbounded iterations | Resource exhaustion | Set iteration limits |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`

## Sub-Layer Launch

When this phase requires step-level execution, launch the
`remediation_procedure` agent. Procedure agents carry the correction
mechanics under 
- `ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure@workflows/reference/academic_revision.md`
- `ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure@workflows/reference/correction_execution.md`
- `ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure@workflows/reference/correction_planning.md`
- `ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure@workflows/reference/issue_analysis.md`
.

Brief template for the `remediation_procedure` agent — Execute <remediation-step> within remediation phase:

```text
<ctxmod_activate skill="remediation_procedure" />
Step: <remediation-step>
<procedure-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Available procedure agents in the remediation domain: dispatch against
the correction variants in 
- `ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure@workflows/reference/academic_revision.md`
- `ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure@workflows/reference/correction_execution.md`
- `ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure@workflows/reference/correction_planning.md`
- `ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure@workflows/reference/issue_analysis.md`

(issue-analysis,
correction-planning, correction-execution).
