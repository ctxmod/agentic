---
description: "Step-level correction mechanics — issue analysis, correction planning, and correction execution procedures, plus academic revision for academic-source remediation."
binds_to_depth: procedure
---

# Remediation Procedures

Execute a single, bounded remediation procedure and return its result.
Each procedure below is a self-contained set of prerequisites, steps,
and decision points; the variant material lives in this skill's
`reference/` directory.

Remediation is a **procedure-rooted partial domain**: there is no
parent `remediation-workflow` skill (decision 2 — sparse cell omitted).
Procedure contexts dispatch directly into this skill.

## Procedure Selection

Read the reference file matching the assigned task. Do not read all
four — each is independently applicable and reading extras wastes
context.

| Assigned task | Read |
|---------------|------|
| Transform validation findings into actionable understanding | `reference/issue_analysis.md` |
| Convert issue analysis into an executable correction plan | `reference/correction_planning.md` |
| Execute correction plan systematically with verification and rollback | `reference/correction_execution.md` |
| Systematic multi-pass revision of an academic document | `reference/academic_revision.md` |

### Issue Analysis

Transform validation findings into actionable understanding by
categorizing issues, identifying root causes, and assessing correction
complexity. Produces a structured analysis summary that correction
planning consumes.

For issue analysis, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure/reference/issue_analysis.md" />

### Correction Planning

Convert issue analysis into an executable correction plan with proper
ordering, dependency management, and verification checkpoints.
Produces a phased correction plan with rollback strategy.

For correction planning, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure/reference/correction_planning.md" />

### Correction Execution

Execute correction plan systematically, verifying each checkpoint
before proceeding and maintaining rollback capability throughout.
Produces a completion report documenting each correction's outcome.

For correction execution, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure/reference/correction_execution.md" />

### Academic Revision (conditional)

**Applies only when** the artifact under remediation is an academic
document (research paper, thesis, scholarly article, formal technical
report) and the validation findings concern prose quality, argument
structure, or stylistic consistency rather than code or configuration
correctness.

For academic-source revision, the issue-analysis → correction-planning
→ correction-execution flow is replaced by the four-pass academic
revision procedure (structure and logic, sentence variety, word choice
and precision, engagement and neutrality). Do not apply academic
revision to non-academic artifacts.

For academic revision, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure/reference/academic_revision.md" />

## Procedure Selection by Finding Class

Not every remediation requires all four procedures. Match procedure
selection to finding class.

| Finding Class | Recommended Procedures |
|---------------|------------------------|
| Code or config defects, structural errors | Issue analysis, correction planning, correction execution |
| Academic document prose issues | Academic revision |
| Mixed (code + documentation prose) | Issue analysis for code; academic revision for prose — run separately |
| Single targeted defect, clear fix | Correction execution (skip analysis and planning) |
| Unclear root cause | Issue analysis before any correction |

## Decision Points

**Findings span multiple artifact types**: Decompose by type. Run the
code/config correction flow (analysis → planning → execution) and the
academic revision flow as separate procedures; their preconditions and
verification methods do not mix.

**Correction plan depends on unresolved analysis**: Return to issue
analysis before executing. Plans built on incomplete understanding
produce unreliable corrections.

**Execution reveals plan invalidity**: Pause execution, rollback to
last verified checkpoint, and return to correction planning with the
newly discovered context.

**Academic revision findings limited to one pass**: All four passes
are still required. Passes are sequenced (structure → sentence → word
→ engagement); skipping a pass invalidates the passes that follow.

## Execution Discipline

- Read only the variant matching the assigned task; do not read all
  four.
- Execute only the assigned procedure; do not expand into adjacent
  steps.
- Report results to the caller-provided path; return a summary, not
  the full body.
- Reference findings with `file:line` ranges rather than inlining
  content.
- Note explicitly when a procedure's prerequisites are unmet rather
  than improvising a substitute.
- For academic revision, complete each pass fully before starting the
  next; do not interleave.

## Success Criteria

- Findings analyzed with root causes and complexity ratings
- Correction plan produced with ordered phases, checkpoints, and rollback
- Corrections executed with verification at each checkpoint
- Completion report documents all outcomes including unresolved findings
- For academic revision: all four passes completed in order with
  verification checklist satisfied

## Common Issues

**Skipped issue analysis**: Corrections address symptoms rather than
root causes, producing recurring findings on re-validation.

**Plan without rollback**: Failures mid-execution leave artifacts in
inconsistent states. Every checkpoint needs a rollback target.

**Execution without verification**: Corrections appear successful but
introduce new issues. Verification must cover correctness, not just
completion.

**Academic revision applied to non-academic artifacts**: The
four-pass procedure assumes prose artifacts. Applying it to code or
configuration produces nonsensical results.

**Interleaving academic passes**: Pass 1 (structure) must stabilize
before Pass 2 (sentence variety); restructuring invalidates
sentence-level work. Interleaving creates rework.

## Workflow Context

This procedure serves the remediation phase skill. There is no
`remediation-workflow` skill (decision 2 — sparse cell omitted);
remediation contexts dispatch directly into the phase.

the `remediation_phase` skill

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
