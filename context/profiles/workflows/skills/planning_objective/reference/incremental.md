# Incremental Planning Objective

Plan changes as staged increments with intermediate verification and adaptation points. Incremental planning produces delivery sequences where each increment yields a verifiable result, and feedback from earlier increments informs later ones.

## Base Behavior

the `planning_objective` skill

## Scope

Incremental planning applies when:

- Requirements may evolve as initial increments reveal new information
- Delivery benefits from intermediate verification before committing to later steps
- Risk reduces through staged rollout rather than single-pass execution
- Dependencies between changes create natural delivery boundaries
- Rollback to a known-good state must remain feasible at each stage
- The full scope spans multiple artifacts or domains with varying uncertainty

## Core Behavior

1. **Decompose**: Break the planned scope into self-contained increments
2. **Order**: Sequence increments by dependency, risk, and information value
3. **Define gates**: Establish verification criteria at each increment boundary
4. **Plan adaptation**: Identify decision points where later increments may change
5. **Map rollback**: Define reversion paths for each increment
6. **Report**: Write incremental plan to the reserved report path
7. **Summarize**: Return brief summary with report path

## Relationship to Other Planning Types

| Type | Delivery | Adaptation |
|------|----------|------------|
| General | Single approach, continuous | Minimal—predetermined path |
| Phased | Multi-phase with gates | Gate-based—adjust between phases |
| **Incremental** | **Staged with feedback** | **Continuous—each increment informs next** |
| Risk-Aware | Risk-driven sequencing | Risk-triggered contingencies |
| Constraint-Based | Boundary-optimized | Constraint-reactive adjustments |

Incremental planning differs from phased planning in granularity and feedback frequency. Phased plans divide work into large phases with formal gate reviews. Incremental plans divide into smaller, independently verifiable units where learning from one unit shapes the next.

## Increment Design

### Increment Properties

Each increment must satisfy these properties:

- **Self-contained**: Produces a verifiable result independent of later increments
- **Reversible**: Can roll back without corrupting prior state
- **Informative**: Reveals information that reduces uncertainty for subsequent increments
- **Bounded**: Has explicit scope, success criteria, and time expectation

### Increment Sizing

Size increments to balance verification overhead against risk exposure. Small increments increase verification cost but limit the scope of potential issues. Large increments reduce overhead but delay feedback. Target the smallest increment size that produces meaningful verification signal.

Sizing considerations vary by artifact type: a documentation increment might cover one section or chapter, a specification increment might address one requirement group, a configuration increment might target one environment, and a research increment might cover one source category.

### Vertical Slices

A **tracer bullet** is the work-item form of an increment: a narrow
but complete path through every layer the change touches (schema,
API, UI, tests), never a horizontal slice of a single layer. The
vertical rules refine the increment properties above:

- **Complete path**: each slice cuts through every layer, so the
  behavior it delivers is reachable end to end
- **Independently verifiable**: a completed slice demonstrates or
  verifies on its own, without the slices that follow
- **One context window**: size each slice to fit a single fresh
  context window; split it when it cannot
- **Prefactor first**: make the change easy, then make the easy
  change — any prefactoring is its own earlier increment

Each work item declares its **blocking edges** — the items that must
complete before it can start — in the item's `blockedBy` frontmatter.
An item whose blockers are all complete may start; the startable set
is the working frontier.

### Wide Refactors: Expand–Contract

Wide refactors are the exception to vertical slicing. A **wide
refactor** is one mechanical change — rename a column, retype a
shared symbol — whose blast radius fans across the codebase, so a
single edit breaks every call site at once and no vertical slice can
land complete. Sequence it as **expand–contract** instead:

1. **Expand**: add the new form beside the old so nothing breaks
2. **Migrate**: move call sites in batches sized by blast radius (per
   package, per directory), each batch its own work item blocked on
   the expand, each batch landing green because the old form still
   exists
3. **Contract**: delete the old form once no caller remains, in an
   item blocked by every migrate batch

When even the batches cannot stay green alone, keep the sequence but
let the batches share an integration branch that all block a final
integrate-and-verify item; green is promised only there.

### Ordering Strategy

Sequence increments to maximize early information gain:

1. High-uncertainty increments first--resolve unknowns before committing resources
2. Foundation increments before dependent ones--respect structural dependencies
3. High-value increments early--deliver measurable progress sooner
4. Isolatable increments in parallel when verification infrastructure permits

## Feedback Mechanisms

### Verification Gates

Define what to verify after each increment completes:

- **Correctness**: Does the increment achieve its stated outcome?
- **Integration**: Does it interact correctly with existing state?
- **Stability**: Does prior functionality remain intact?
- **Assumptions**: Were planning assumptions for this increment valid?

### Adaptation Triggers

Identify conditions that require adjusting the remaining plan:

| Trigger | Response |
|---------|----------|
| Verification failure | Diagnose, fix, re-verify before proceeding |
| Assumption invalidated | Reassess affected increments, resequence if needed |
| New dependency discovered | Insert prerequisite increment or reorder |
| Scope change from findings | Add, remove, or modify remaining increments |
| Effort estimate exceeded | Resize remaining increments or adjust scope |

### Decision Points

Mark explicit points where the plan branches:

- After increment N, evaluate whether increment N+1 proceeds as planned or requires modification
- Document the criteria that determine each branch
- Include the "stop" option—when accumulated findings indicate the remaining plan is no longer viable

## Rollback Strategy

### Per-Increment Rollback

Each increment defines its own rollback path:

- **Pre-state snapshot**: What to preserve before the increment executes
- **Reversion steps**: How to undo the increment's changes
- **Verification**: How to confirm rollback restored prior state
- **Cascade effects**: Whether rollback affects earlier increments (it should not)

### Progressive Commitment

Early increments are low-commitment—easy to reverse. As increments accumulate, reverting the entire sequence becomes costlier. The plan should identify the point of diminishing reversibility and treat it as a key decision gate.

## Pre-Incremental Planning

Before planning increments:

1. Read analysis reports to understand the full scope and recommended approaches
2. Identify natural decomposition boundaries in the target scope
3. Assess which elements carry the highest uncertainty
4. Determine verification capabilities available between increments
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

## During Incremental Planning

Plan each increment with these guidelines:

- **Explicit boundaries**: State what each increment includes and excludes
- **Success criteria per increment**: Define verifiable outcomes, not activity completion
- **Dependency declaration**: List what each increment requires from prior increments
- **Adaptation latitude**: Indicate which aspects of later increments may change based on feedback
- **Rollback cost**: Estimate the effort to reverse each increment

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/checkpoint_definition.md`

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`

## Verification

After completing the plan:

1. Confirm each increment is self-contained with independent success criteria
2. Verify the increment sequence respects all dependencies
3. Check that rollback paths exist for every increment
4. Validate that adaptation triggers cover likely deviation scenarios
5. Ensure the plan identifies the point of diminishing reversibility

## Tool Constraints

**Allowed**:
- Read: Analysis reports, reference files, related artifacts
- Grep: Search for dependency and constraint information
- Glob: Locate relevant artifacts for scope assessment
- Write: Planning report at the reserved report path only

**Forbidden**:
- Modifying artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Making implementation changes

## Response Format

After writing the report, return a brief response:

```
Wrote incremental planning report to <reserved report path>.

Summary: [2-3 sentences: number of increments, key ordering rationale, primary adaptation triggers]
```

Do not return full report content. The report file is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Increments without verification gates | Loses feedback benefit | Define success criteria per increment |
| Irreversible early increments | Eliminates course correction | Front-load reversible changes |
| Uniform increment sizing | Ignores varying uncertainty | Size by risk and information value |
| Rigid later increments | Wastes adaptation opportunity | Mark adaptation latitude explicitly |
| Skipping rollback planning | Trapped in failed state | Define reversion path per increment |
| Sequential-only ordering | Missed parallelization | Identify independent increments |
| Feedback without adaptation | Observation without action | Define triggers and responses |

## Related

the `planning_objective` skill

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/checkpoint_definition.md`

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/risk_assessment.md`

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
