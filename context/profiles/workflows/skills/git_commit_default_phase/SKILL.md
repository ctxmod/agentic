---
description: "Default git-commit phase — suggest commits, run blocking precommit validation, execute only when validation passes."
binds_to_depth: phase
---

# Git Commit Default Phase

Default git-commit phase — suggest commits, run blocking precommit
validation, and execute commits only when validation passes. This is
one of three phase variants of the git-commit capability (default,
safe, unsafe); they share the same objective (`git_commit_objective`)
and procedure (`git-commit-procedure`) layers and differ only in
orchestration parameters.

Git-commit is a **phase, not a workflow**. The main agent invokes it
directly via the absorbed `/git:commit` slash-command, or any other
domain's workflow agent references this phase as a "commit your
changes" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/patterns_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflows_overview.md" />

## Story Completion Routing

Story completion does not route through this phase: it routes to the
story-completion validator (Haiku-class), which validates, fixes,
rechecks, and commits in one session. The traced commit path from a
completed story therefore needs no separate suggest, validate, and
execute agents — the validator performs the whole cycle under the
acting validator contract, and if it cannot complete the cycle it
returns a blocker and creates no success claim.

### Validator commit contract

The validator's commit follows the repository's own commit workflow
and the ctxmod builtin `commit_discipline` profile;
the acting validator contract lives in
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />.
Skill-level requirements:

- **Commit-scope byte identity**: the committed bytes must equal the
  bytes the validator approved. Record a pre-stage manifest (the exact
  story path set plus content identities), stage by explicit story
  paths — never a bare `git add -A` / `git add .` — and prove the
  committed versions against the manifest immediately after the commit.
- **Peer exclusion**: pre-staged or concurrently edited peer files are
  excluded even when present in the same worktree index.
- **Unrelated-dirty tolerance**: unrelated modified or untracked files
  may remain before and after the commit; whole-tree cleanliness is
  not a gate.
- **Commit standards and guard enforcement stand**: atomic scope,
  technical message text (never work-item IDs), and the repository's
  guards apply unchanged — ordinary sub-agents remain gated against
  committing, and the host commit guard still enforces the same rules
  on the validator's commit.
- **Success record**: commit identifier, scoped check-gate result,
  deliberately run package-test result set, deliberately run test-tool
  result set, and commit-byte proof. The validator may conclude
  "scoped quality gates pass"; it may conclude "real-world behavior
  works" only when a relevant test tool was deliberately exercised.
  Package tests (selected by name through the check gate, landed) and
  test tools (live) are the deliberate test-execution surfaces.

## When to Use

Use this phase when:

- Repository contains uncommitted changes
- Quality validation should gate commits
- Automated commit organization improves consistency
- Routine commits with clear changes

This phase provides the default balance of automation and quality gates.

**When not to use**:

- Story completion commits — those belong to the story-completion
  validator (see Story Completion Routing above), not to this chain.

**When to use alternatives**:
- `git_commit_safe_phase` for complex changes needing human review of strategy
- `git_commit_unsafe_phase` to bypass blocking validation (emergency hotfix, false-positive hooks)

## Phase Orchestration

This phase dispatches three objective-depth sub-agents in sequence.
The same objective (`git_commit_objective`) runs each time, with
different variants and parameters. This chain serves explicit commit
requests only — user-invoked `/git:commit` or a workflow's "commit
your changes" sub-step. Story completion never follows it: the
story-completion validator commits directly (see Story Completion
Routing above).

```
Phase 1: Commit Suggestion
         ↓  (VALIDATION_MODE=blocking prep)
Phase 2: Commit Validation (blocking)
         ↓  (passes only if no manual-fix issues)
Phase 3: Commit Execution (validated)
         ↓
      Complete
```

### Phase 1: Commit Suggestion

Dispatch `git_commit_objective` with the **suggest-commits** variant.
The objective analyzes repository state, recent commit patterns, and
work-item context, then writes 1-3 atomic commit strategies to the
report path.

- VALIDATION_MODE: n/a (suggest-commits is read-only)
- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-suggest-commits.md`

Suggestion methodology emphasizes:
- Repository pattern analysis (analyze recent history before assuming conventions)
- 1-3 strategies with rationale and trade-offs
- Work-item context via the work store (`ctxmod work list`, `ctxmod work show`, and a body search for changed paths)

### Phase 2: Commit Validation

Dispatch `git_commit_objective` (or directly the `precommit` procedure)
to validate staged files. **VALIDATION_MODE=blocking**.

- Apply auto-fixes automatically, re-stage files, re-validate.
- **Halt on manual-fix issues** — do not proceed to execution.
- Report validation status in completion.

If validation halts (manual-fix required), report blockers and exit
without creating commits. Auto-fixes are applied and re-validated
automatically without human intervention.

### Phase 3: Commit Execution

Dispatch `git_commit_objective` with the **make-commits** variant.
**EXECUTION_MODE=validated** — execute only after Phase 2 passes.

- Execute the primary recommended strategy in dependency order
  (dependencies before dependents).
- Document commit hashes and file lists.
- Use commit messages from the recommendation report verbatim.

## Information Flow

```
Phase 1: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-suggest-commits.md
Phase 2: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-commit-validation.md
Phase 3: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-commit-execution.md
```

Main agent passes report **paths** between phases, never report
content. Phase 3 reads the suggestion report path as INPUT_REPORT.

## Error Handling

| Situation | Response |
|-----------|----------|
| No uncommitted changes | Report and exit (nothing to do) |
| Suggestion produces no strategies | Report and exit |
| Validation finds manual-fix issues | Halt, report blockers, create no commits |
| Validation auto-fixes applied | Re-stage, re-validate, proceed if clean |
| Execution fails | Capture error, report partial state |

Error handling follows stop-report-propose-await pattern. Never use
`--no-verify` to bypass blocking failures — document them instead.

## Quality Criteria

| Criterion | Validation Method | Phase |
|-----------|-------------------|-------|
| Atomic commits | Phase 1 strategy organizes by logical concern | 1 |
| Quality standards | Phase 2 validation enforces hooks | 2 |
| Validation gates | Phase 3 executes only after validation passes | 3 |

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Routing story completion through the three-dispatch chain | Story completion commits via the story-completion validator in one session |
| Gating a commit on whole-tree cleanliness | Unrelated-dirty tolerance: gate by commit scope, not tree state |
| Proceeding past validation failures | Halt on manual-fix issues |
| Inlining report content | Pass report paths between phases |
| Single strategy when alternatives exist | Present multiple strategies in suggestion |
| Ignoring repository conventions | Reference commit history in suggestion |
| Using `--no-verify` to skip blocking failures | Document and exit without committing |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`git_commit_objective` agent. The objective in turn launches
`git-commit-procedure` for step mechanics.

Brief template for the `git_commit_objective` agent — Execute <variant> within git-commit default phase:

```text
<ctxmod_activate skill="agentic_workflows_git_commit_objective" />
Variant: <suggest-commits|make-commits>
Phase-specific instructions:
  - VALIDATION_MODE=blocking
  - EXECUTION_MODE=validated
<objective-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Dispatch variants per phase position:

| Phase | Variant | Procedures launched |
|-------|---------|---------------------|
| Phase 1 (suggestion) | suggest-commits | commit-recommendations |
| Phase 2 (validation) | suggest-commits (validation scope) | precommit |
| Phase 3 (execution) | make-commits | commits + precommit |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"commit your changes" as a sub-step. Launch the `git_commit_default_phase`
agent from any workflow that needs blocking-validation commits.

Brief template for the `git_commit_default_phase` agent — Execute git commit default phase:

```text
<ctxmod_activate skill="agentic_workflows_git_commit_default_phase" />
Execute the full suggest → validate (blocking) → execute chain.
Story completion does not route here — it belongs to the
story-completion validator.
```

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile
`push_plan_safety` in the ctxmod builtin `git_safety` profile
