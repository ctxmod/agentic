---
description: "Two-gate git-commit phase — suggest commits, present for human approval, run blocking validation, execute only approved commits; re-confirm after auto-fixes."
binds_to_depth: phase
---

# Git Commit Safe Phase

Two-gate git-commit phase — suggest commits, present strategies for
human approval, run blocking precommit validation, and execute only
approved commits. Two quality gates protect this phase: human judgment
and automated validation. Both must pass before commits execute.

This is one of three phase variants of the git-commit capability
(default, **safe**, unsafe). The safe variant inserts a human-approval
gate between suggestion and validation, and re-confirms approval when
auto-fixes modify approved files.

Git-commit is a **phase, not a workflow**. The main agent invokes it
directly via the absorbed `/git_commit_safe` slash-command, or any
other domain's workflow agent references this phase as a "commit with
human oversight" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/patterns_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflows_overview.md" />

## When to Use

Use this phase when:

- Multiple valid commit strategies exist
- Human judgment improves commit organization
- Cross-module changes require scoping judgment
- High-stakes changes benefit from oversight

Two quality gates protect this phase: human approval and precommit
validation. Both must pass before commits execute.

**When to use alternatives**:
- `git_commit_default_phase` for routine commits where automated validation suffices
- `git_commit_unsafe_phase` to bypass blocking validation in emergencies

## Phase Orchestration

This phase dispatches objective-depth sub-agents in sequence, with a
human-approval gate executed by the main agent between Phase 1 and
Phase 3.

```
Phase 1: Commit Suggestion (2-3 strategies)
         ↓
Phase 2: Human Approval (main agent, no sub-agent)
         ↓
Phase 3: Commit Validation (blocking)
         ↓  (re-confirm if auto-fixes modify approved files)
Phase 4: Commit Execution (validated)
         ↓
      Complete
```

### Phase 1: Commit Suggestion

Dispatch `git_commit_objective` with the **suggest-commits** variant.

- Emphasize **multiple strategies** when valid alternatives exist —
  generate 2-3 strategies to enable informed human choice.
- Provide clear rationale and trade-offs for each.
- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-suggest-commits.md`

### Phase 2: Human Approval

**Main agent executes directly (no sub-agent).** Present recommendation
summary with alternatives, await developer input (y/n/number), generate
APPROVED_APPROACH record, abort workflow if developer rejects all
options.

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/human_approval.md`

### Phase 3: Commit Validation

Dispatch validation with **VALIDATION_MODE=blocking**.

- Apply auto-fixes automatically.
- **Re-confirm approval if auto-fixes modify files included in the
  approved strategy** — return to Phase 2 for re-confirmation before
  executing commits. This prevents commits that differ from what was
  originally approved.
- Halt on manual-fix issues.
- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-commit-validation.md`

### Phase 4: Commit Execution

Dispatch `git_commit_objective` with the **make-commits** variant.
**EXECUTION_MODE=validated** — execute approved strategy only.

- Execute approved strategy only.
- Document deviations from recommendations in report.
- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-commit-execution.md`

## Information Flow

```
Phase 1: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-suggest-commits.md
Phase 2: APPROVED_APPROACH (passed to Phase 3 and 4)
Phase 3: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-commit-validation.md
Phase 4: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-commit-execution.md
```

Main agent passes report paths and APPROVED_APPROACH between phases.

## Variants

**Auto-Fix Re-confirmation Loop** (when validation applies auto-fixes
that modify approved files):

```
Approval → Validation → Auto-fixes Applied → Re-confirm Approval → Execution
```

Present modified files to developer. If declined, abort without
committing.

**Binary Approval** (for simple changes with clear strategy):

```
Suggestion → Yes/No Approval → Validation → Execution
```

Developer approves or rejects single recommendation.

## Error Handling

| Situation | Response |
|-----------|----------|
| Developer rejects all strategies | Abort workflow, no commits created |
| Auto-fixes modify approved files | Re-confirm approval before proceeding |
| Validation finds manual-fix issues | Halt, report blockers, await resolution |
| Developer declines after auto-fixes | Abort without committing |

## Quality Criteria

| Criterion | Validation Method | Phase |
|-----------|-------------------|-------|
| Multiple options considered | Phase 1 generates 2-3 strategies | 1 |
| Human judgment applied | Phase 2 approval required | 2 |
| Quality standards enforced | Phase 3 validation gates | 3 |
| Approved strategy executed | Phase 4 follows approval | 4 |

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Proceeding without approval | Require Phase 2 approval |
| Skipping re-confirmation after auto-fixes | Re-confirm when files modified |
| Presenting single option as choice | Generate genuine alternatives or use default phase |
| Over-approval for trivial changes | Use default phase for routine commits |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`git_commit_objective` agent. The objective in turn launches
`git-commit-procedure` for step mechanics.

Brief template for the `git_commit_objective` agent — Execute <variant> within git-commit safe phase:

```text
<ctxmod_activate skill="agentic_workflows_git_commit_objective" />
Variant: <suggest-commits|make-commits>
Phase-specific instructions:
  - VALIDATION_MODE=blocking
  - EXECUTION_MODE=validated
  - HUMAN_APPROVAL_REQUIRED=true
  - Generate 2-3 strategies for human choice (suggest variant)
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Variant | Procedures launched |
|-------|---------|---------------------|
| Phase 1 (suggestion) | suggest-commits (2-3 strategies) | commit-recommendations |
| Phase 2 (approval) | (main agent, no sub-agent) | n/a |
| Phase 3 (validation) | suggest-commits (validation scope) | precommit |
| Phase 4 (execution) | make-commits (approved only) | commits + precommit |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"commit changes with human oversight" as a sub-step. Launch the
`git_commit_safe_phase` agent from any workflow needing a human gate.

Brief template for the `git_commit_safe_phase` agent — Execute git commit safe phase:

```text
<ctxmod_activate skill="agentic_workflows_git_commit_safe_phase" />
Execute the suggest → approve → validate (blocking) → execute chain.
```

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />
`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/human_approval.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile
`push_plan_safety` in the ctxmod builtin `git_safety` profile
