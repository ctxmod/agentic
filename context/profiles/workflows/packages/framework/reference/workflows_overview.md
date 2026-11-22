# Orchestration Workflows

Workflows define complete multi-phase plans for high-level activities. They compose patterns and objectives into structured execution paths.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Workflow vs Pattern vs Objective

| Concept | Scope | Question Answered |
|---------|-------|-------------------|
| **Objective** | Sub-agent | What does this agent accomplish? |
| **Pattern** | Coordination | How do agents coordinate? |
| **Workflow** | Activity | How do we accomplish this high-level goal? |

## Workflow Reference

**Artifact workflows**:

| Workflow | Purpose | Phases | Primary Pattern |
|----------|---------|--------|-----------------|
| Writing | Create structured artifacts | Discovery → Plan → Create → Validate | Sequential + validation loop |
| Implementation | Modify existing artifacts | Discover → Analyze → Modify → Verify | Sequential + verification loop |
| Review | Assess artifact quality | Scope Discovery → Parallel Validation → Synthesis → Remediation Planning | Parallel + fan-in synthesis |
| Migration | Transform artifact state | Baseline → Target → Gap → Plan → Transform → Verify → Complete | Phased sequential + checkpoints |

**Analysis workflows**:

| Workflow | Purpose | Phases | Primary Pattern |
|----------|---------|--------|-----------------|
| Gap | Identify missing elements vs requirements | Discovery (conditional) → Target Resolution → Gap Analysis → Prioritization | Sequential + prioritization |
| Compare | Evaluate alternatives for decisions | Parallel Discovery → Criteria → Parallel Evaluation → Synthesis | Fan-out fan-in |
| Quality | Multi-dimensional quality assessment | Discovery → Parallel Assessment → Synthesis → Planning | Parallel + synthesis |
| Debt | Technical debt inventory and prioritization | Parallel Discovery → Impact → Prioritization → Roadmap | Parallel + roadmap planning |
| Comprehensive | Multi-dimensional analysis combining all dimensions | Scope Resolution → Parallel Discovery → Parallel Assessment → Aggregation → Synthesis | Fan-out fan-in + aggregation |

**Work item and execution workflows**:

| Workflow | Purpose | Phases | Primary Pattern |
|----------|---------|--------|-----------------|
| Work Item | Create epics, stories, issues, tasks | ID Resolution → Discovery → Writing → Validate | Sequential + auto-ID |
| Precommit | Validate via precommit hooks, create work items, execute | Discovery → Work Item Creation (conditional) → Execution → Validation | Sequential + conditional work items |
| Spec | Validate specifications with research-backed corrections | Discovery → Parallel Validation → Analysis → Research (conditional) → Synthesis → Planning → Work Items → Correction → Verification Loop | Parallel validation/research + verification loop |

## Selection Guide

The Workflow Reference above maps directly to most activities. For ambiguous cases:

| Activity | Recommended Workflow | Notes |
|----------|---------------------|-------|
| Fix identified issues | Implementation | Skip discovery when issues known |
| Document findings | Writing | Lightweight variant if structure predetermined |
| Audit compliance | Review | Multi-criteria parallel validation |
| Upgrade conventions | Migration | Phased with rollback checkpoints |

### Activity Decomposition

Complex activities combine workflows:

| Composite Activity | Sequence |
|-------------------|----------|
| New artifact from existing ones | Review → Writing |
| Comprehensive quality improvement | Review → Implementation → Review |
| Migration with new artifacts | Migration → Writing (for gaps) → Review |

## Workflow Details

### Writing

**Variants**: Lightweight (skip planning when structure predetermined), Research-heavy (fan-out discovery), Multi-artifact (parallel creation of related artifacts)

### Implementation

**Variants**: Lightweight (skip analysis for well-specified changes), Multi-scope (fan-out for independent modifications), Isolated (clean-HEAD checkouts — clone or archive+init — for parallel conflicting changes), Incremental (staged with intermediate verification)

### Review

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_review.md" />

**Variants**: Focused (single criterion), Iterative (include remediation and re-validation), Comparative (before/after assessment), Scoped parallel (per-scope assessment with cross-scope synthesis)

### Migration

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_migration.md" />

**Variants**: Lightweight (combined phases for small migrations), Parallel scope (concurrent independent scopes), Incremental (staged rollout with inter-increment verification), Reversible (shadow state for parallel operation)

### Work Item

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_work_item.md" />

**Variants**: Lightweight (skip analysis for clear scope), Epic with decomposition (epic then spawned story workflows), Batch (multiple tasks sharing discovery context)

**Note**: Writing phase always uses Opus for work items.

### Gap Analysis

`ctx:github.com/ctxmod/agentic/context::shared::analyze_gap_phase@workflows/reference/gap.md`

**Variants**: Direct (skip discovery when input report provided), Requirements-only (target from work item acceptance criteria)

### Compare Analysis

`ctx:github.com/ctxmod/agentic/context::shared::analyze_compare_phase@workflows/reference/compare.md`

**Variants**: Criteria-driven (explicit criteria provided), Two-alternative (binary decisions with implicit criteria)

### Quality Analysis

`ctx:github.com/ctxmod/agentic/context::shared::analyze_quality_phase@workflows/reference/quality.md`

**Variants**: Single-dimension (one quality aspect), Focused check (assessment without planning), Comprehensive audit (four+ dimensions with planning)

### Debt Analysis

`ctx:github.com/ctxmod/agentic/context::shared::analyze_debt_phase@workflows/reference/debt.md`

**Variants**: Focused category (single debt type), Quick inventory (assessment without roadmap), Strategic planning (full workflow with detailed roadmap)

### Comprehensive Analysis

`ctx:github.com/ctxmod/agentic/context::shared::analyze_comprehensive_phase@workflows/reference/aggregation.md`

**Variants**: Targeted (dimension subset), Research-heavy (external evidence phase), Iterative (staged assessment)

**Note**: Comprehensive synthesis uses Opus for complex multi-dimensional reasoning.

### Precommit

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_work_precommit.md" />

**Variants**: Auto-fix only (skip work items when all issues resolved), Append to existing story, New story under epic

### Spec

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_work_spec.md" />

**Variants**: No research needed (skip research when no questionable approaches), No issues found (complete after validation), Auto-fix only (corrections without work items), Research-heavy (full research with synthesis)

## Composition

### Pattern Embedding

| Workflow Phase | Common Pattern |
|----------------|----------------|
| Multi-source discovery | Fan-Out Fan-In |
| Independent validations | Parallel |
| Quality verification | Validation Loop |
| Phased transformation | Sequential |
| Results aggregation | Fan-In |

### Nesting

Workflows may nest for complex activities. Keep nesting to 2 levels maximum:

```
Migration Workflow
└── Phase 5: Transformation
    └── Implementation Workflow (per transformation phase)
        └── Validation Loop (within modification phase)
```

### Common Combinations

```
Quality Improvement:  Review → Implementation → Review
Artifact Lifecycle:   Writing → Review → Implementation → Review
System Transition:    Review (baseline) → Migration → Review (target)
```

## Shared Conventions

### Parameters

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

Work item context: EPIC_PATH, STORY_PATH, ISSUE_PATH, TASK_PATH. Report output: report paths reserved just-in-time by the executing sub-agent (`ctxmod work new report "<title>"`, with `--owner` set to the work item when one exists). Phase chaining: INPUT_REPORT passes previous phase output.

### Error Handling

All workflows: stop on failure (do not proceed to dependent phases), report what failed with context, propose options (retry, scope reduction, manual intervention), await user decision. Workflow-specific handling documented in individual workflow files.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping discovery | Uninformed actions | Gather context first |
| Skipping verification | Unconfirmed outcomes | Verify before completion |
| Mixing workflows | Unclear execution | Complete one before starting another |
| Deep nesting | Complexity explosion | 2 levels maximum |
| Ignoring phase outputs | Lost context | Chain outputs as inputs |
| Proceeding after failure | Compounding errors | Stop, assess, decide |

## Related

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/patterns_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
