# Phased Planning Objective

Decompose complex changes into sequential phases with inter-phase validation gates. Phased planning applies when execution spans multiple distinct stages that depend on prior stage completion, require intermediate verification, or involve separate responsible parties.

## Base Behavior

the `planning_objective` skill

## Scope

Phased planning applies when:

- Execution requires multiple distinct stages with verifiable intermediate states
- Later work depends on results or validation from earlier stages
- Risk management demands isolation between stages
- Different agents or processes handle different portions of work
- Rollback to an intermediate state must remain possible if later phases fail

Use general planning when a single approach suffices without staging. Use incremental planning when requirements may evolve between iterations rather than following a predetermined sequence.

## Phased Planning Focus

### Phase Decomposition

Divide execution into phases where each phase produces a verifiable intermediate state. Phases represent meaningful boundaries--points where partial progress has standalone value or where assumptions from earlier work require validation before proceeding.

Effective decomposition criteria:

- **Dependency boundaries**: Later work depends on earlier results
- **Verification requirements**: Intermediate states need confirmation before continuation
- **Risk isolation**: Failures within one phase should not invalidate completed phases
- **Responsibility transitions**: Different agents, teams, or processes handle different phases

### Phase Dependencies

Map how phases relate to each other. Dependencies take three forms:

| Dependency Type | Definition | Example |
|-----------------|------------|---------|
| Sequential | Phase B requires Phase A output | Schema migration before data migration |
| Conditional | Phase B runs only if Phase A meets criteria | Performance optimization only if baseline exceeds threshold |
| Parallel-then-join | Phases B and C run independently, Phase D requires both | Independent component changes merging at integration |

### Phase Gates

Each phase boundary includes a gate--a set of conditions that must hold before the next phase begins. Gates prevent cascading failures by catching problems early. Define gates as binary pass/fail checks with explicit rollback triggers when a gate fails.

Gate components:

- **Pass criteria**: Specific, measurable conditions (all tests pass, all references resolve, schema validates)
- **Verification method**: How to check criteria (automated test, manual review, artifact inspection)
- **Rollback trigger**: What constitutes failure and what action to take
- **Rollback scope**: Whether failure rolls back the current phase only or cascades to earlier phases

## Methodology

### Step 1: Identify Phase Boundaries

Review analysis recommendations and map natural break points. Look for dependency clusters, verification needs, and risk boundaries. Group related steps into candidate phases.

### Step 2: Define Phase Specifications

For each phase, specify:

- **Objective**: What the phase accomplishes (single sentence)
- **Inputs**: What the phase consumes (artifacts, reports, prior phase outputs)
- **Steps**: Ordered execution within the phase
- **Outputs**: What the phase produces (verifiable artifacts)
- **Gate criteria**: Conditions for proceeding to next phase

### Step 3: Map Inter-Phase Dependencies

Build a dependency graph across phases. Identify the critical path--the longest sequential chain determining minimum total duration. Flag phases eligible for parallel execution.

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`

### Step 4: Define Gate Criteria

Establish verification checks at each phase boundary. Gate criteria should be binary (pass/fail), measurable, and automatable where possible. Include rollback triggers specifying what happens when a gate fails.

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/checkpoint_definition.md`

### Step 5: Assess Phase-Level Risks

Evaluate risks within and across phases. Phase-level risks include scope expansion within a phase, gate failure probability, and cross-phase dependency fragility.

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/risk_assessment.md`

### Step 6: Sequence and Optimize

Arrange phases respecting dependencies while maximizing parallelism. Identify phases that can overlap. Establish the execution timeline with phase durations and buffer allocation.

## Relationship to Other Planning Types

| Type | When to Use | Phase Structure |
|------|-------------|-----------------|
| General | Single clear approach | No phases |
| **Phased** | **Multi-stage with dependencies** | **Explicit phases with gates** |
| Incremental | Evolving requirements | Iterative cycles |
| Risk-Aware | High-uncertainty execution | Risk-organized steps |
| Constraint-Based | Tight resource limits | Constraint-bounded steps |

Phased planning differs from incremental planning: phases follow a predetermined sequence with defined gates, while increments adapt based on feedback from prior cycles.

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the phased planning report path just-in-time and create the structured output following the report organization and creation procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

### Phased-Specific Sections

Beyond standard planning report structure, phased plans include:

- **Phase Specifications**: Per-phase objective, inputs, steps, outputs, and gate criteria
- **Dependency Graph**: Inter-phase dependency map with critical path highlighted
- **Gate Definitions**: Verification criteria and rollback triggers at each boundary
- **Phase Timeline**: Execution sequence with duration estimates and parallelism opportunities

## Tool Permissions

**Allowed**: Read (analysis reports, reference), Grep (dependency search), Glob (scope assessment), Write (reserved report path only)

**Forbidden**: Modifying artifacts, launching sub-agents, writing outside the reserved report path, making implementation changes

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Phases without gates | Cascading failures go undetected | Define verification criteria at every boundary |
| Too many phases | Overhead exceeds benefit | Target 3-5 phases; merge small phases |
| Monolithic single phase | Loses phased planning benefit | Decompose at dependency and risk boundaries |
| Implicit dependencies | Execution order violations | Map all inter-phase dependencies explicitly |
| Gates that require full rollback | Disproportionate recovery cost | Design phases for partial rollback where possible |
| Rigid phase ordering without rationale | Missed parallelism | Document why sequential ordering is required |

## Examples

**Documentation Restructure**

Phase 1: Create new directory structure and placeholder files (gate: all target paths exist). Phase 2: Migrate content from old locations to new (gate: no content loss, all sections present). Phase 3: Update cross-references throughout project (gate: all links resolve). Phase 4: Remove old files (gate: no remaining references to old paths).

**Research Literature Review**

Phase 1: Define search criteria and source databases (gate: criteria reviewed, databases accessible). Phase 2: Collect and catalog candidate sources (gate: all sources recorded with metadata). Phase 3: Screen sources against inclusion criteria (gate: screening decisions documented with rationale). Phase 4: Synthesize findings into thematic analysis (gate: all included sources represented, no gaps in coverage).

**Specification Compliance Audit**

Phase 1: Inventory current artifacts against specification requirements (gate: complete mapping exists). Phase 2: Assess compliance for each requirement (gate: all items scored, evidence documented). Phase 3: Prioritize gaps by severity and remediation cost (gate: priority ranking approved). Phase 4: Apply corrections and re-verify (gate: all critical gaps closed, compliance score meets threshold).

**Configuration Migration**

Phase 1: Deploy new format alongside existing (gate: new format validates correctly). Phase 2: Migrate consumers to new format (gate: all dependents updated, no resolution errors). Phase 3: Monitoring period for regressions (gate: zero fallbacks to old format). Phase 4: Remove old format files (gate: clean removal, no orphaned references).

## Related

the `planning_objective` skill

Brief template for a procedure-depth agent (launched from objective depth):

```text
Map inter-phase dependencies and define gate criteria with rollback triggers for each phase boundary.
`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`
`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/checkpoint_definition.md`
```

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
