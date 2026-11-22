# Validation Phase

Verify that artifacts meet defined quality criteria. Validation assesses outcomes against standards, schemas, conventions, and acceptance criteria to determine if work is complete.

## When to Use

Use a validation phase when:

- An explicit request or high-risk scope selects a validation dimension
- Compliance with schemas or conventions must be assessed for an engaged scope
- Quality gates protect downstream consumers of validated artifacts
- Acceptance criteria must be confirmed outside story closure

Routine story completion does not route through these phases: it belongs
to the story-completion validator (Haiku-class), which validates, fixes,
rechecks, and commits in one session; if it cannot complete the cycle it
returns a blocker and creates no success claim. Validation phases are
explicitly selected dimensions, never dispatched by default.

Validation provides quality assurance for explicitly engaged scopes. It
follows execution and precedes completion or remediation.

## Variants

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_phase_selection.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_academic.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_aggregation.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_docs.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_equivalence.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_general.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_multi_criteria.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_precommit.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_scope_discovery.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_standards.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_synthesis.md" />

## Model Selection

**Primary**: Haiku-class story-completion validation

The story-completion validator is the primary validation surface: it
derives touched and related scope, runs deterministic scoped checks,
repairs failures in-session, rechecks every changed byte, and commits in
one session. Comparison against explicit criteria is deterministic work;
a higher tier is not needed for it.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />

**Exceptions**: Higher tiers only for genuinely complex interpretation —
validation where the difficulty is interpreting the criteria themselves
rather than applying them (semantic alignment across a large
cross-artifact surface, ambiguous schema intent). Such engagements are
selected explicitly and named; never promote routine validation to a
higher tier by default.

## Coordination Pattern

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />

Story closure uses the validation loop as one bounded session: the
story-completion validator runs deterministic oracles, repairs
in-session, and rechecks — iterating only while an oracle fails, with no
iteration on a green first pass.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

Parallel dispatch applies only to an explicitly engaged multi-dimension
assessment — one validation agent per independently selected criterion,
launched in a single message. It is never the default shape.

## Context Requirements

**Parameters provided by main agent**:

| Parameter | Required | Purpose |
|-----------|----------|---------|
| Report (reserved) | Yes | Sub-agent creates the report record just-in-time with `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing |
| ARTIFACT_PATHS | Yes | What to validate |
| CRITERIA | Recommended | Validation criteria reference |
| Work item context | If applicable | EPIC_PATH, STORY_PATH, ISSUE_PATH |

## Acceptance Criteria

### Default Thresholds

| Metric | Pass Threshold |
|--------|----------------|
| Errors | 0 |
| Warnings | ≤ 5 |
| Coverage | All required elements present |

### Configurable Thresholds

Main agents may adjust thresholds based on artifact criticality, project maturity, time constraints, or user preferences. Document adjusted thresholds in validation report.

## Transition Criteria

Validation completes and transitions when:

- All artifacts have been assessed
- Findings are classified by severity
- Status determination is clear
- Patterns are identified for remediation guidance

If validation passes, workflow may complete. If validation fails, remediation phase follows. Story closure is different: the story-completion validator repairs in-session within its own one-session cycle rather than handing off to a remediation phase.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Validation without criteria | Subjective assessment | Define criteria before validating |
| Partial criteria coverage | Missed issue classes | Check the full selected criteria set |
| Ignoring warnings | Quality degradation | Address warnings or justify acceptance |
| Validating partial artifacts | Incomplete assessment | Validate complete artifacts |
| Skipping pattern analysis | Miss systemic issues | Identify recurring problems |
