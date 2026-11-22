# Run Workflow

Orchestrates sequential execution of child work items with report passing between siblings.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Workflow Overview

```
Phase 1: Work Item Analysis
         ↓
Phase 2: Status Transition (conditional: draft only)
         ↓
Phase 3: Scope Discovery (parallel, conditional: no recent reports)
         ↓
Phase 4: Research (conditional: approach unclear)
         ↓
Phase 5: Gap Analysis
         ↓
Phase 6: Execution Planning
         ↓
Phase 7: Execution
         ↓
Phase 8: Validation ←──── Phase 9: Remediation (max 3)
         ↓ All pass
Phase 10: Completion Verification
         ↓
Phase 11: Status Completion → Complete
```

## When to Use

- A work item already exists and requires execution
- Completing work from current state to done
- Re-running after a previous attempt stalled or failed
- Orchestrating hierarchical work item completion (epics, stories with children)

Assumes a work item exists. For creating and executing in a single workflow, see the full workflow.

## Phase Structure

### Phase 1: Work Item Analysis

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_general.md" />

**Workflow-specific notes**:
- Main agent launches a single Sonnet sub-agent
- Sub-agent reads the work item definition and any existing reports
- Extracts type, status, scope, completion criteria, child items, blockers
- Output determines which subsequent phases execute and which skip
- If work item is blocked, main agent halts immediately and reports the blocker

This phase always executes. Its output governs the conditional logic for every subsequent phase.

### Phase 2: Status Transition

`ctx:github.com/ctxmod/agentic/context::shared::validate_structure_phase@workflows/reference/structural.md`

**Workflow-specific notes**:
- **Conditional**: Execute when `status == open` (draft); skip when already `in_progress` (active); halt when `blocked`
- **Parent status automation**: If parent is `open`, transition parent to `in_progress` first
- Sonnet sub-agent verifies readiness (required sections, parent `in_progress`/`done`) and sets the status from `open` to `in_progress` (`ctxmod work set <id> status=in_progress`)

### Phase 3: Scope Discovery

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_multi_source.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

**Workflow-specific notes**:
- **Conditional**: Skip when recent discovery reports exist (24h threshold, relevant scope)
- **Parallel**: Two Sonnet sub-agents in a single message — architecture discovery (components, boundaries, interfaces, patterns) and dependencies discovery (inbound/outbound, consumers, cross-cutting concerns)
- Both receive Phase 1 analysis; fan-in at Phase 5 gap analysis

### Phase 4: Research

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_general.md" />

**Workflow-specific notes**:
- **Conditional**: Execute when approach is unclear or work item specifies research needs
- **Skip**: Exact implementation specified, purely structural work, or similar recent research exists
- Sonnet sub-agent uses available search tools to research relevant practices
- Output feeds into gap analysis alongside discovery reports

### Phase 5: Gap Analysis

`ctx:github.com/ctxmod/agentic/context::shared::analyze_gap_phase@workflows/reference/gap.md`

**Workflow-specific notes**:
- Always executes. Consumes discovery reports, research report (if produced), and work item definition
- Sonnet sub-agent maps each completion criterion to current state: fully met, partially met, or not met
- Produces prioritized gap list with complexity estimates per gap
- Gaps determine the scope of Phases 6 and 7

This is the convergence point where all preceding information merges into a single actionable assessment.

### Phase 6: Execution Planning

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/general.md`

**Workflow-specific notes**:
- Always executes. Sonnet sub-agent consumes gap analysis to produce a change plan
- Plans minimal changes that close identified gaps
- Sequences actions by dependency, groups related changes for efficiency
- Defines verification checkpoints for Phase 7
- Identifies parallelization opportunities within execution
- Output: execution plan with artifact paths, change descriptions, sequence, and rollback strategy

### Phase 7: Execution

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_edit.md" />

**Workflow-specific notes**:
- Always executes. Opus sub-agent applies planned changes with checkpoints between logical change units
- Receives: execution plan, discovery reports, research report (if applicable)
- Scope boundary: apply planned changes only; do not expand beyond plan
- **Hierarchical types**: Execution may involve creating child items and recursively orchestrating their completion. Use `phases/execution/general.md` instead of `phases/execution/edit.md` when scope expands beyond edits.

### Phase 8: Validation

the `validation_objective` skill
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_multi_criteria.md" />

**Workflow-specific notes**:
- Always executes. Story completion is the one mandatory high-level validation boundary: it routes to the story-completion validator (Haiku-class), one session that validates against deterministic oracles, repairs scoped failures — including proven pre-existing ones in touched or related packages — rechecks every changed byte, and commits with commit-scope byte identity
- Between phases, the main agent runs risk-based local checks — cheap, deterministic, in its own tree, scaled to the risk of the change; a failed local check halts the workflow
- Parallel subjective validator fan-out across specialized dimensions (structure, references, consistency, completeness) is available only when explicitly selected for the story's artifacts or risk — never a routine default
- Issues found route to Phase 9 (remediation loop); a clean pass proceeds to Phase 10

### Phase 9: Remediation Loop

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />
`ctx:github.com/ctxmod/agentic/context::shared::remediation_phase@workflows/reference/targeted.md`

**Workflow-specific notes**:
- **Conditional**: Triggers when any Phase 8 validation reports issues
- **Maximum 3 iterations**, each following the linked validation-loop pattern with re-validation returning to Phase 8
- Sonnet handles correction planning (cost-effective analysis); Opus handles correction execution (modification quality)
- **Exit on exhaustion**: Halt, preserve all artifacts and reports, provide actionable guidance for manual resolution

### Phase 10: Completion Verification

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_general.md" />

**Workflow-specific notes**:
- Always executes after validation passes
- Sonnet sub-agent checks: all completion criteria mapped to evidence, all children resolved (completed/cancelled) for hierarchical types, no unresolved blockers
- Output: completion eligibility verdict (yes/no) with rationale

### Phase 11: Status Completion

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_edit.md" />

**Workflow-specific notes**:
- **Conditional**: Execute only when Phase 10 determines eligibility is `yes`
- **Skip**: When Phase 10 determines `no` (main agent reports unmet criteria and recommends next actions)
- Sonnet sub-agent sets the status from `in_progress` to `done` (`ctxmod work set <id> status=done`)
- Changes no other field (state lives in the record's state file; the body is untouched)
- Single artifact modification

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
<work item dir>/reports/<YYYY-MM>/
  <DD-HHMM>-<id>-00-analysis.md                    Phase 1
  <DD-HHMM>-<id>-01-status-transition.md           Phase 2 (conditional)
  <DD-HHMM>-<id>-02a-discovery-architecture.md     Phase 3 (parallel, conditional)
  <DD-HHMM>-<id>-02b-discovery-dependencies.md     Phase 3 (parallel, conditional)
  <DD-HHMM>-<id>-03-research.md                    Phase 4 (conditional)
  <DD-HHMM>-<id>-04-gap-analysis.md                Phase 5
  <DD-HHMM>-<id>-05-execution-plan.md              Phase 6
  <DD-HHMM>-<id>-06-execution.md                   Phase 7
  <DD-HHMM>-<id>-07-validation.md                  Phase 8 (story-completion validator)
  <DD-HHMM>-<id>-08a-correction-plan-{N}.md        Phase 9 (per iteration)
  <DD-HHMM>-<id>-08b-correction-{N}.md             Phase 9 (per iteration)
  <DD-HHMM>-<id>-09-completion-verification.md     Phase 10
```

All reports live within the work item directory. Main agent passes report paths between phases, never report content.

## Execution Strategy

### Main Agent Behavior

1. **Resolve work item**: Identify from explicit parameter, conversation context, or current directory
2. **Phase 1** (always): Analyze work item → determines conditional phase execution
3. **Phases 2-6** (sequential, conditional): Skip conditions per Conditional Phase Execution table
4. **Phase 7** (Opus): Execute planned changes
5. **Phase 8**: Story completion routes to the story-completion validator (Haiku-class)
6. **Decision**: Issues found → Phase 9 remediation loop (max 3, returns to Phase 8); all pass → Phase 10
7. **Decision**: Phase 10 eligible → Phase 11 status completion; not eligible → report unmet criteria

### Parameter Configuration

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## Work Item Type Adaptations

Execution varies by work item type. The phase structure remains identical; the content within phases adapts.

| Type | Discovery Focus | Research Need | Execution Pattern | Completion Condition |
|------|-----------------|---------------|-------------------|----------------------|
| Epic | Strategic context, market positioning | Deep research common | Story decomposition and orchestration | All stories completed or cancelled |
| Story | Feature context, technical patterns | Best practices typical | Issue decomposition and orchestration | All issues completed or cancelled |
| Issue | Implementation context, affected artifacts | Specific patterns as needed | Direct changes or task decomposition | Completion criteria met, tasks resolved |
| Task | Specific artifacts, bounded scope | Minimal or none | Direct changes only | Acceptance criteria met |

### Recursive Orchestration

Epics, stories, and issues with child items trigger recursive execution following a strict sequence:

1. **Parent Phases 2-6**: Parent transitions from `open` (draft) to `in_progress` (active) (Phase 2), discovers child items and relationships (Phase 3), plans orchestration including child decomposition (Phases 4-6)
2. **Parent Phase 7**: Launches child run workflows sequentially. Each child receives all previous sibling reports as compulsory reads. Dependencies execute first, then remaining children by priority or natural ordering.
3. **Child execution**: Each child runs its own full workflow (including its own Phase 2 status transition)
4. **Parent Phases 10-11**: Parent completes after all children reach terminal state (completed or cancelled)

A child failure does not halt other children; the main agent reports child status and continues. The recursive pattern handles status transitions automatically — no manual `/lifecycle_start` commands before running child workflows.

## Conditional Phase Execution

| Phase | Executes When | Skips When |
|-------|---------------|------------|
| Phase 2: Status Transition | `status == open` (draft) | Already `in_progress` (active) or halts if `blocked` |
| Phase 3: Scope Discovery | No recent discovery reports (24h threshold) | Recent comprehensive reports exist |
| Phase 4: Research | Approach unclear or work item requests research | Exact implementation specified, purely structural work |
| Phase 9: Remediation Loop | Any Phase 8 validation fails | All validations pass |
| Phase 11: Status Completion | Phase 10 determines `eligible: yes` | Phase 10 determines `eligible: no` |

Phases 1, 5, 6, 7, 8, and 10 always execute.

## Workflow Variants

### Direct Execution

Skip discovery and research for well-understood work. Phase 1 finds recent discovery reports and approach is already specified; Phases 3 and 4 skip via conditional logic without configuration.

### Verification-Heavy Execution

Emphasize validation for high-risk changes: expand Phase 8 with additional validation dimensions (functional verification, regression detection), apply stricter remediation criteria (zero warnings, not just zero errors), and add custom validation sub-agents beyond the explicitly selected set.

### Incremental Execution

Stage execution with intermediate verification. Phase 7 executes a planned subset, Phase 8 verifies that subset, and the cycle repeats for each stage before final completion verification. Use when the change set benefits from staged application.

### Resume Execution

Continue from a previous stalled or failed attempt. Phase 1 inventories existing reports and determines progress; phases with completed reports skip automatically. Execution resumes from the earliest incomplete phase, with accumulated reports from prior invocations available to subsequent phases.

## Error Handling

| Situation | Response | Recovery |
|-----------|----------|----------|
| Work item blocked | Report blocker details, halt immediately | Resolve blocker, re-invoke `/run` |
| Remediation loop exhausted (3 iterations) | Report remaining issues, preserve all artifacts and reports | Manual correction then re-invoke `/run`, or accept with known issues |
| Completion verification fails | Report unmet criteria, recommend specific next actions | Address criteria, re-invoke `/run` |
| Child item blocked or failed | Report child status, continue with other children | Address child separately, re-invoke parent `/run` |
| Missing dependencies | Report dependency, halt affected phases | Acquire dependency, re-invoke `/run` |
| Scope exceeds single-session capacity | Report progress, propose decomposition | Decompose into smaller work items |

Error handling follows the stop-report-propose-await pattern: halt at failure point, report full context, propose recovery options, await user decision. All progress persists in reports regardless of failure.

## Quality Criteria

| Criterion | Validation Method | Phase |
|-----------|-------------------|-------|
| Completion criteria satisfaction | Phase 8 maps each criterion to execution evidence | 8, 10 |
| Structural integrity | Phase 8 validates all modified artifacts against format and schema | 8 |
| Reference validity | Phase 8 verifies all links resolve, no orphaned references | 8 |
| Child item resolution | Phase 10 confirms all children completed or cancelled | 10 |
| No unresolved blockers | Phase 10 checks blocker field absent or resolved | 10 |
| Scope containment | Phase 8 verifies no unintended changes outside scope | 8 |

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Reading intermediate reports into main agent context | Violates context efficiency, bloats orchestrator | Pass report paths between phases |
| Skipping validation for "simple" work | Hidden issues propagate to completion | Always execute Phases 8-10 |
| Expanding scope during remediation | Scope creep, iteration exhaustion | Fix only issues identified by validation |
| Proceeding past a blocked status | Child items inherit blocked state | Halt immediately, report blocker |
| Ignoring recent discovery reports | Redundant work, wasted context budget | Reuse discovery from within 24 hours |
| Running all phases when resuming | Repeats completed work | Check for existing reports, skip completed phases |
| Serializing explicitly selected validation fan-out when concurrent dispatch is granted | Wastes turns and time | Launch the selected validators in a single message |
| Concurrent dispatch without the `allow-parallel-agents` grant | Conflicting work, lost context inheritance | Run the same validators one at a time |
