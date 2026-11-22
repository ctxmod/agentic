# Migration Workflow

Orchestrate artifact transitions from current state to target state through phased discovery, planning, transformation, and verification. Migration workflows produce systematic state transitions with rollback capability.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Workflow Overview

Migration workflows follow a comprehensive sequential structure:

```
Phase 1: Baseline Discovery
         ↓
Phase 2: Target Definition
         ↓
Phase 3: Gap Analysis
         ↓
Phase 4: Migration Planning
         ↓
Phase 5: Phased Transformation ←──┐
         ↓                        │
Phase 6: Verification             │
         ↓                        │
    Issues Found? ────Yes─────────┘
         ↓ No
Phase 7: Completion
         ↓
      Complete
```

## When to Use

Use migration workflow when:

- Artifacts require systematic transformation to new state
- Current and target states differ significantly
- Transformation benefits from phased execution
- Rollback capability is essential
- Progress checkpoints aid risk management

Migration workflows apply to any state transition: schema migrations, format conversions, convention adoptions, structure reorganizations.

## Phase Structure

### Phase 1: Baseline Discovery

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_general.md" />

**Workflow-specific notes**:
- Document current state comprehensively before any changes
- Serves as rollback reference point
- Discovery variants: source, architecture, dependencies, patterns

### Phase 2: Target Definition

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_general.md" />

**Workflow-specific notes**:
- Establish clear target state specifications
- May infer target from baseline and requirements
- Output: Target state specification with success criteria

### Phase 3: Gap Analysis

`ctx:github.com/ctxmod/agentic/context::shared::analyze_gap_phase@workflows/reference/gap.md`

**Workflow-specific notes**:
- Compare baseline to target
- Categorize differences by type and complexity
- Output: Gap analysis report with transformation requirements

### Phase 4: Migration Planning

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/migration.md`

**Workflow-specific notes**:
- Create phased transformation plan with rollback points
- Define checkpoints and dependencies
- Output: Migration plan with phases, dependencies, checkpoints

### Phase 5: Phased Transformation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_edit.md" />

**Workflow-specific notes**:
- Execute migration plan in defined phases
- Implementation variants: refactor, edit, create, delete, rename
- Potential parallel execution within phases
- For migrations with independent scopes that can transform in parallel, use `phases/execution/parallel.md` instead

### Phase 6: Verification

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_general.md" />
`ctx:github.com/ctxmod/agentic/context::shared::remediation_phase@workflows/reference/targeted.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />

**Workflow-specific notes**:
- Validation loop within each transformation phase
- Verify transformation achieves target state
- Remediate issues before proceeding to next phase

### Phase 7: Completion

the `write_report_phase` skill

**Workflow-specific notes**:
- Finalize migration and document outcomes
- Produce migration completion report
- Summary of transformations, issues, recommendations

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Phase 1: Baseline Discovery
- Receives: {inherited work item context}
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-baseline.md

Phase 2: Target Definition
- Receives: {inherited}, Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-target.md

Phase 3: Gap Analysis
- Receives: {inherited}, Phase 1 output, Phase 2 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-gap-analysis.md

Phase 4: Migration Planning
- Receives: {inherited}, Phase 3 output
- Available: Phase 1, Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-migration-plan.md

Phase 5: Phased Transformation (per phase N)
- Receives: {inherited}, Phase 4 output
- Available: Previous transformation reports
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-05-transform-phase-{N}.md

Phase 6: Verification (per phase N)
- Receives: {inherited}, transformed artifacts
- Available: Phase 2 output (target criteria)
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-06-verify-phase-{N}.md

Phase 7: Completion
- Receives: {inherited}, all phase reports
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-07-completion.md
```

## Execution Strategy

### Main Agent Behavior

1. **Baseline**: Launch discovery sub-agent(s); wait for completion
2. **Target**: Launch target definition; wait for completion
3. **Gap Analysis**: Launch gap analysis; wait for completion
4. **Planning**: Launch migration planning; wait for completion
5. **Transform Loop** (for each migration phase):
   a. Launch transformation sub-agent(s)
   b. Wait for completion
   c. Launch verification sub-agent
   d. If issues found: Correct or rollback
   e. If clean: Proceed to next phase
6. **Completion**: Launch completion documentation; return summary

### Parameter Configuration

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## Workflow Variants

### Lightweight Migration

Combine phases when:
- Migration scope is limited
- Target state is well-defined
- Gap is small and understood

```
Discovery → Gap Analysis → Transformation → Verification → Completion
```

### Parallel Scope Migration

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

Use Fan-Out Fan-In when:
- Multiple independent scopes require migration
- Scopes can transform in parallel
- Final verification assesses all scopes together

```
Discovery (Fan-Out)         Transformation (Fan-Out)         Verification
├── Scope A baseline ───────┬── Scope A transform ──────────┐
├── Scope B baseline ───────┼── Scope B transform ──────────┼──→ Integration verify
└── Scope C baseline ───────┴── Scope C transform ──────────┘
```

### Incremental Migration

Use staged rollout when:
- Risk requires gradual transition
- Verification between increments is essential
- Partial migration provides value

```
For each increment:
  Discovery → Transform Subset → Verify → Checkpoint
Until all migrated
```

### Reversible Migration

Use shadow state when:
- Rollback capability is critical
- Parallel operation during transition required
- Validation requires comparison to baseline

```
Baseline → Target Definition → [Transform + Shadow] → Compare → Cutover
                                    ↑                    ↓
                                    └── Rollback ←── Issues Found
```

## Checkpoints and Rollback

### Checkpoint Strategy

Establish checkpoints at:
- After baseline discovery (restore point)
- After each transformation phase
- Before and after critical operations
- At verification milestones

### Checkpoint Content

Each checkpoint documents:
- Artifacts in scope at this point
- Changes applied since last checkpoint
- Verification status
- Rollback instructions

### Rollback Procedures

When rollback is needed:

1. **Identify rollback point**: Most recent clean checkpoint
2. **Assess scope**: What changes to revert
3. **Execute rollback**: Apply inverse transformations or restore from checkpoint
4. **Verify rollback**: Confirm return to known-good state
5. **Document failure**: Record what failed and why

## Error Handling

### Baseline Discovery Failure

If baseline discovery fails:

1. Report discovery failure
2. Cannot proceed without baseline—await user intervention
3. Propose scope reduction or manual baseline creation

### Gap Analysis Failure

If gap analysis cannot identify differences:

1. Report analysis difficulties
2. Check baseline and target clarity
3. Propose manual gap specification or scope reduction

### Transformation Failure

If transformation fails during execution:

1. Stop transformation immediately
2. Assess partial state
3. Execute rollback to last checkpoint
4. Report failure with context
5. Propose alternative approach or manual intervention

### Verification Failure (Exceeded Iterations)

If verification loop exceeds maximum iterations:

1. Report persistent issues
2. Assess whether to proceed to next phase or stop
3. Document unresolved issues
4. Propose manual correction or alternative approach
5. Await user decision

## Quality Criteria

### Phase-Level Criteria

Each transformation phase verifies:
- Artifacts transformed match plan
- No unintended changes outside scope
- References remain intact
- Phase-specific success criteria met

### Migration-Level Criteria

Overall migration verifies:
- All gaps addressed
- Target state achieved
- No regressions from baseline
- Cross-phase consistency maintained

### Configurable Criteria

Main agents may adjust based on migration type:

| Migration Type | Additional Criteria |
|----------------|---------------------|
| Schema migration | Data integrity, backward compatibility |
| Format conversion | Lossless transformation, encoding correctness |
| Convention adoption | Pattern consistency, no convention violations |
| Structure reorganization | Behavior preservation, dependency integrity |

## Risk Mitigation

### Pre-Migration

- **Comprehensive baseline**: Document everything before changes
- **Clear target**: Unambiguous success criteria
- **Phased plan**: Manageable increments with checkpoints
- **Rollback strategy**: Defined procedures for each phase

### During Migration

- **Phase isolation**: Complete phases before starting next
- **Verification gates**: No progression without passing verification
- **Incremental progress**: Prefer small verified steps
- **Documentation**: Record each transformation

### Post-Migration

- **Final verification**: Full target state compliance check
- **Baseline comparison**: Confirm intended changes, no regressions
- **Cleanup**: Remove temporary artifacts, shadow states
- **Documentation**: Complete migration report

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping baseline | No rollback point | Always document current state |
| Big-bang transformation | High risk | Phase the migration |
| Ignoring verification | Unconfirmed state | Verify each phase |
| No checkpoints | Difficult rollback | Checkpoint between phases |
| Unclear target | Ambiguous success | Define target explicitly |
| Proceeding after failures | Compounding problems | Stop, rollback, reassess |
