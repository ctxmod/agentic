---
description: "Pull request review with change discovery, multi-criteria validation, and approval recommendation."
binds_to_depth: phase
---

# Review PR Phase

Pull request review phase — extract the changeset from a PR
identifier, run parallel validation across structural, standards,
consistency, and reference criteria, synthesize findings, and
classify an approval recommendation (Approve / Request Changes /
Comment).

Review PR is a **phase, not a workflow**. The main agent invokes it
directly via the `/review_pr` slash-command.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_review.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

## When to Use

Use this phase when:

- Reviewing a pull request before merge
- Validating changes against repository standards
- Producing an actionable approval recommendation with line-level comments
- Distinguishing blocking issues from suggestions

- the /review command selection guidance — when the full /review gate
  is warranted versus this specialized variant — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/review.md" />

## Phase Orchestration

This phase dispatches workflow-depth sub-agents across four phases.
The review workflow reference carries the per-phase shape.

```
Phase 1: Change Discovery (Sonnet)
         ↓
Phase 2: Multi-Criteria Validation (Sonnet, parallel)
         ├── Structural validation
         ├── Standards validation
         ├── Consistency validation
         ├── Reference validation
         └── Spec Fidelity validation (conditional)
         ↓
Phase 3: Findings Synthesis (Sonnet)
         ↓
Phase 4: Recommendation (Sonnet)
         ↓
      Complete
```

### Phase 1: Change Discovery (Sonnet)

Launch the `workflow` agent for change discovery:

Brief template for a workflow-depth agent — PR change discovery:

```text
Execute the change-discovery phase (extract changed files from PR_IDENTIFIER).
PR_IDENTIFIER: <user-provided identifier>
```

**Command-specific notes**:
- SCOPE = PR changeset identified by PR_IDENTIFIER
- Extract changed files, additions, deletions, modifications
- Identify artifact types affected
- Note PR description and linked issues
- Use git diff and PR metadata for change extraction
- Pin the changeset base before any Phase 2 dispatch: capture the
  changeset diff command once in merge-base form (`git diff <fp>...HEAD`)
  plus the commit list (`git log <fp>..HEAD --oneline`); verify the base
  resolves (`git rev-parse <fp>`) and the diff is non-empty — fail loud
  in discovery, never inside parallel validators
- When the PR references a work item, resolve it now (see Spec Fidelity
  under Validation Criteria) so the conditional fifth axis can launch
- Categorize changes by type (artifact, test, config, docs)

### Phase 2: Multi-Criteria Validation (Sonnet, parallel)

Launch the resolved validation axes in parallel — four by default, five
when Phase 1 resolved a work item:

Brief template for a workflow-depth agent — Structural validation of PR changes:

```text
Execute the structural validation phase on changed artifacts.
```

**Command-specific notes**:
- All validators focus on changed artifacts only, not entire repository
- Phase 1 discovery report provides artifact paths for validation
- Spec Fidelity launches only when Phase 1 resolved a work item; when it
  did not, that axis is skipped and synthesis reports "no work item
  available"
- Fan-in expects four OR five reports accordingly

### Phase 3: Findings Synthesis (Sonnet)

Launch the synthesis agent:

Brief template for a workflow-depth agent — PR findings synthesis:

```text
Execute the synthesis phase (fan-in of all Phase 2 validation reports).
```

**Command-specific notes**:
- Fan-in aggregation of all Phase 2 validation reports
- Aggregate per axis under per-axis headings; never merge or rerank
  findings across axes
- End with a one-line per-axis summary carrying the worst finding within
  each axis; no single cross-axis winner
- When the Spec Fidelity axis was skipped, note "no work item available"
- Distinguish blocking issues from suggestions
- Consolidate severity across validators
- Assess change completeness (missing tests, docs)

### Phase 4: Recommendation (Sonnet)

Launch the recommendation agent:

Brief template for a workflow-depth agent — PR recommendation:

```text
Execute the recommendation phase (verdict classification).
```

**Command-specific notes**:
- RECOMMENDATION_FORMAT = verdict
- Classify as: Approve, Request Changes, or Comment
- Include specific line comments for issues
- Prioritize blocking issues
- Acknowledge positive aspects of changes
- Reference governing work-item requirements if available — the Spec
  Fidelity axis (Phase 2) is the systematic home for requirement checks

## Information Flow

```
Phase 1: Discovery
- Receives: pr-identifier
- Produces: changeset inventory

Phase 2: Validation (parallel)
- Receives: Phase 1 inventory
- Produces: per-criterion validation reports
- Conditional: the Spec Fidelity axis runs only when Phase 1 resolved a
  work item; fan-in expects four OR five reports

Phase 3: Synthesis
- Receives: all Phase 2 reports
- Produces: consolidated findings

Phase 4: Recommendation
- Receives: Phase 3 synthesis
- Produces: verdict + line-level comments
```

Main agent passes report **paths** between phases, never report content.

## Recommendation Outcomes

| Outcome | Criteria |
|---------|----------|
| Approve | No errors, warnings acceptable, changes complete |
| Request Changes | Errors present, blocking issues identified |
| Comment | Suggestions only, no blocking issues |

## Validation Criteria

| Criterion | Focus |
|-----------|-------|
| Structural | Format and schema compliance in changed artifacts |
| Standards | Convention adherence in modifications |
| Consistency | Alignment with existing repository patterns |
| References | Link integrity after changes |
| Spec Fidelity | Faithful implementation of the originating work item (conditional — runs only when Phase 1 resolved a work item) |

Spec Fidelity findings are three classes: (a) requirements the work
item asked for that are missing or partial; (b) behavior in the
changeset that was not asked for (scope creep); (c) requirements that
look implemented but wrongly. Resolve the work-item source in this
order: work-item references in commit messages → a user-supplied path →
a file matching the branch or feature name → ask the user. If nothing
resolves, skip the axis; synthesis reports "no work item available".

The Standards axis carries the judgement-call smell baseline beneath
documented standards: a documented repo standard overrides the
baseline, each smell is a labelled judgement call rather than a hard
violation, and anything tooling enforces is skipped. The full 12-smell
catalog: <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::review/reference/review_smell_baseline.md" />.

## Why Two Axes

A changeset can pass one axis and fail the other: code that follows
every standard while implementing the wrong thing (Standards pass,
Spec Fidelity fail), and code that does exactly what the work item
asked while breaking the repo's conventions (Spec Fidelity pass,
Standards fail). Reporting the axes separately stops one axis from
masking the other.

## Error Handling

| Situation | Response |
|-----------|----------|
| PR identifier unresolved | Halt, request valid identifier |
| Diff base unresolvable | `git rev-parse` fails — fail loud in Phase 1 discovery, request a valid base |
| Empty changeset | Fail loud in Phase 1 — report and exit before any Phase 2 dispatch (nothing to review) |
| Validation reports conflict | Synthesis resolves via severity consolidation |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Reviewing entire repository | Focus validators on changed artifacts only |
| Serializing Phase 2 validators | Run all resolved axes in parallel |
| Approval without line-level feedback | Always include actionable line comments |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`validation_objective` or `analysis_objective` agents per phase.

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"review a PR" as a sub-step. Launch the `review_pr_phase` agent from
any workflow that needs pull request assessment.

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile
