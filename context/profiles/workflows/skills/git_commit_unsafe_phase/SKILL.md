---
description: "Emergency git-commit phase — suggest commits, run informational validation, execute regardless of findings with risk-acceptance documentation."
binds_to_depth: phase
---

# Git Commit Unsafe Phase

Emergency git-commit phase — suggest commits, run validation
informationally (documents findings, does not block), and execute
commits regardless of validation status. Annotates commits with
validation findings, includes a "Known Issues" section, and recommends
remediation follow-up.

This is one of three phase variants of the git-commit capability
(default, safe, **unsafe**). The unsafe variant changes both validation
mode (informational) and execution mode (unsafe — proceeds regardless
of findings).

Git-commit is a **phase, not a workflow**. The main agent invokes it
directly via the absorbed `/git_commit_unsafe` slash-command, or any
other domain's workflow agent references this phase as an "emergency
commit" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/patterns_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflows_overview.md" />

## When to Use

Use this phase when:

- Emergency hotfixes require speed over validation
- Precommit hooks produce false positives
- Time-critical situations with known acceptable issues
- Validation findings are informational but should not block

Validation runs and reports findings, but those findings are
informational only and never prevent commit execution.

**When to use alternatives**:
- `git_commit_default_phase` for routine commits where validation should gate execution
- `git_commit_safe_phase` for complex changes needing human review

## Phase Orchestration

This phase dispatches three objective-depth sub-agents in sequence.
The validation objective runs in informational mode (documents, does
not block); the execution objective runs in unsafe mode (annotates
commits, proceeds regardless).

```
Phase 1: Commit Suggestion
         ↓
Phase 2: Commit Validation (informational)
         ↓  (findings documented, do not block)
Phase 3: Commit Execution (unsafe)
         ↓
      Complete
```

### Phase 1: Commit Suggestion

Dispatch `git_commit_objective` with the **suggest-commits** variant.

- Analyze repository state, recommend commit organization.
- Identify associated work items.
- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-suggest-commits.md`

### Phase 2: Commit Validation

Dispatch validation with **VALIDATION_MODE=informational**.

- Document all findings but **do not block execution**.
- Apply auto-fixes if available, report results.
- Skip manual-fix resolution (findings documented only).
- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-commit-validation.md`

### Phase 3: Commit Execution

Dispatch `git_commit_objective` with the **make-commits** variant.
**EXECUTION_MODE=unsafe** — execute commits regardless of validation
status.

- Annotate each commit with validation status.
- Include "Known Issues" section for each commit.
- Add "Risk Acceptance" section noting unsafe variant.
- Recommend follow-up actions for remediation.
- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-commit-execution.md`

## Information Flow

```
Phase 1: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-suggest-commits.md
Phase 2: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-commit-validation.md
Phase 3: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-commit-execution.md
```

Main agent passes report paths between phases.

## Risk Mitigation

This phase includes several risk-mitigation strategies:

- **Documentation**: All validation findings prominently documented.
- **Commit annotations**: Each commit annotated with validation status.
- **Follow-up actions**: Recommended remediation steps in execution report.
- **Visibility**: "Known Issues" section ensures issues are not hidden.

## Variants

**With Human Approval** (insert human approval between suggestion and
validation — for high-stakes unsafe commits):

```
Suggestion → Human Approval → Validation (informational) → Execution (unsafe)
```

Provides a human-judgment gate while still executing regardless of
validation findings. (Use `git_commit_safe_phase` instead if you also
want blocking validation.)

**Auto-Fix and Proceed** (when auto-fixes are available):

```
Validation → Auto-fixes Applied → Proceed with Known Issues
```

Apply fixes that are safe, document remaining issues, proceed.

## Error Handling

| Situation | Response |
|-----------|----------|
| Validation produces findings | Document and proceed |
| Auto-fixes available | Apply, document, proceed |
| Execution fails | Capture error, report partial state |

Unlike the default phase, validation findings do not halt execution.

## Quality Criteria

| Criterion | Validation Method | Phase |
|-----------|-------------------|-------|
| Atomic commits | Phase 1 strategy organizes by logical concern | 1 |
| Issues documented | Phase 2 reports all findings | 2 |
| Risk accepted | Phase 3 annotates commits with known issues | 3 |

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Using unsafe for routine commits | Reserve for emergencies or false positives |
| Ignoring validation findings | Document all issues prominently |
| Skipping documentation | Annotate commits with validation status |
| No follow-up plan | Recommend remediation steps |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`git_commit_objective` agent. The objective in turn launches
`git-commit-procedure` for step mechanics.

Brief template for the `git_commit_objective` agent — Execute <variant> within git-commit unsafe phase:

```text
<ctxmod_activate skill="git_commit_objective" />
Variant: <suggest-commits|make-commits>
Phase-specific instructions:
  - VALIDATION_MODE=informational
  - EXECUTION_MODE=unsafe
  - Annotate commits with validation status
  - Include "Known Issues" section in execution report
  - Add "Risk Acceptance" section noting unsafe variant
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Variant | Procedures launched |
|-------|---------|---------------------|
| Phase 1 (suggestion) | suggest-commits | commit-recommendations |
| Phase 2 (validation) | suggest-commits (informational scope) | precommit (informational) |
| Phase 3 (execution) | make-commits (unsafe mode) | commits + precommit (informational) |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"emergency commit" as a sub-step. Launch the `git_commit_unsafe_phase`
agent from any workflow needing commits that proceed regardless of
validation findings.

Brief template for the `git_commit_unsafe_phase` agent — Execute git commit unsafe phase:

```text
<ctxmod_activate skill="git_commit_unsafe_phase" />
Execute the suggest → validate (informational) → execute (unsafe) chain.
```

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile
`push_plan_safety` in the ctxmod builtin `git_safety` profile
