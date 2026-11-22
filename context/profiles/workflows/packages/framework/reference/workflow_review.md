# Review Workflow

Orchestrate comprehensive artifact assessment through multi-criteria validation, analysis synthesis, and remediation planning. Review workflows are explicit engagements — requested by the user or mandated by a separate risk policy — not a routine step in ordinary work. Routine story verification is owned by the story-completion validator (Haiku-class, one session); do not dispatch this workflow for ordinary closure.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Workflow Overview

Review workflows combine multi-criteria validation with sequential synthesis:

```
Phase 1: Scope Discovery
         ↓
Phase 2: Multi-Criteria Validation (criteria selected by risk)
         ├── Criterion A validation
         ├── Criterion B validation
         └── Criterion C validation
         ↓
Phase 3: Findings Synthesis (Fan-In)
         ↓
Phase 4: Remediation Planning
         ↓
      Complete
```

## When to Use

Use review workflow when ALL of these hold:

- The review is explicit: the user requested it, or a separate risk
  policy mandates it
- Artifacts require assessment against multiple criteria
- Findings need synthesis into unified recommendations
- Remediation guidance is expected as output

Review workflows apply to any artifact assessment: quality audits, compliance checks, readiness reviews, gap assessments. When only one or two criteria carry material risk, validate those — a fixed five-validator fan-out is not the shape; select validators the way the story-completion validator selects oracles: by risk and acceptance criteria.

## Phase Structure

### Phase 1: Scope Discovery

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_general.md" />

**Workflow-specific notes**:
- Identify review targets and establish evaluation context
- Produce artifact inventory and review parameters
- Sequential pattern for scope establishment

### Phase 2: Multi-Criteria Validation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_multi_criteria.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

**Workflow-specific notes**:
- Criteria are selected by risk for this review, not a fixed fan-out — launch validators only for the criteria that carry material risk for the target
- Each validation type runs independently on same artifacts
- Validation variants: structural, standards, completeness, references, consistency, functional

### Phase 3: Findings Synthesis

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_synthesis.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

**Workflow-specific notes**:
- Fan-in aggregation of parallel validation outputs
- Cross-report pattern extraction
- Prioritized findings with severity consolidation

### Phase 4: Remediation Planning

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/general.md`

**Workflow-specific notes**:
- Generate actionable guidance for addressing findings
- May produce work items for remediation tracking
- Prioritized actions based on synthesis

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Phase 1: Scope Discovery
- Receives: {inherited work item context}
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery.md

Phase 2: Multi-Criteria Validation (Parallel)
- Receives: {inherited}, Phase 1 output
- Produces:
  - {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-structural.md
  - {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-standards.md
  - {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-completeness.md
  - {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-val-references.md

Phase 3: Findings Synthesis
- Receives: {inherited}, all Phase 2 outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-synthesis.md

Phase 4: Remediation Planning
- Receives: {inherited}, Phase 3 output
- Available: Phase 2 outputs (detail reference)
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-remediation.md
```

## Execution Strategy

### Main Agent Behavior

1. **Plan**: Define review criteria and expected validation dimensions
2. **Discovery**: Launch discovery sub-agent; wait for completion
3. **Parallel Validation**: Launch the selected validation sub-agents (criteria chosen by risk)
4. **Wait**: Monitor all validation agents until complete
5. **Synthesis**: Launch synthesis sub-agent with all validation reports
6. **Planning**: Launch remediation planning sub-agent with synthesis output
7. **Complete**: Return review summary with report paths

### Parameter Configuration

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## Workflow Variants

### Focused Review

Use single-criterion validation when:
- Review targets one specific quality dimension
- Full multi-criteria assessment is unnecessary
- Speed matters more than comprehensiveness

```
Discovery → Single Validation → Remediation Planning
```

### Iterative Review

the `remediation_phase` skill
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />

Add validation loop when:
- Remediation should be verified
- Review includes correction phase
- Quality gate must be passed

```
Discovery → Validation (Parallel) → Synthesis → Remediation ←─┐
                                                       ↓      │
                                          Apply Corrections   │
                                                       ↓      │
                                                Re-Validate ──┘
```

### Comparative Review

Use comparative analysis when:
- Multiple artifact versions require comparison
- Before/after assessment is needed
- Baseline comparison informs findings

```
Discovery (Baseline) ─┐
                      ├──→ Comparative Validation → Synthesis → Planning
Discovery (Current) ──┘
```

### Scoped Parallel Review

Use Fan-Out Fan-In by scope when:
- Different artifact groups require separate review
- Groups can be assessed independently
- Findings need cross-group synthesis

```
Discovery → Validation (By Scope)            Synthesis
             ├── Scope A validation ──────────┐
             ├── Scope B validation ──────────┼──→ Unified findings → Planning
             └── Scope C validation ──────────┘
```

## Error Handling

### Discovery Failure

If discovery fails to identify review targets:

1. Report discovery failure
2. Check if partial inventory is sufficient
3. Propose scope reduction or manual target specification
4. Await user decision

### Validation Agent Failure

If some validation agents fail:

1. Continue waiting for remaining agents
2. Collect successful validation outputs
3. Document missing criteria in synthesis
4. Flag synthesis limitations due to incomplete validation
5. Proceed or await user decision based on failure criticality

### Synthesis Failure

If synthesis cannot produce coherent findings:

1. Report synthesis difficulties
2. Present individual validation reports directly
3. Propose simplified aggregation or manual synthesis
4. Await user decision

## Quality Criteria

### Default Review Criteria

The candidate criteria most reviews draw from — select the subset that
carries material risk for the target:

| Criterion | Focus |
|-----------|-------|
| Structural | Format and schema compliance |
| Standards | Convention adherence |
| Completeness | Required element coverage |
| References | Link and dependency integrity |

### Extended Review Criteria

Main agents may add criteria based on review purpose:

| Purpose | Additional Criteria |
|---------|---------------------|
| Compliance audit | Regulatory requirements |
| Readiness review | Deployment prerequisites |
| Quality gate | Acceptance criteria |
| Gap assessment | Target state comparison |

## Synthesis Techniques

### Pattern Aggregation

Identify findings appearing across multiple validation reports:
- **High frequency** (3+ reports): Systemic issues
- **Medium frequency** (2 reports): Cross-cutting concerns
- **Single occurrence**: Localized issues

### Severity Consolidation

Aggregate severity across validation dimensions:

| Condition | Consolidated Severity |
|-----------|----------------------|
| Error in any criterion | Error |
| Warnings across 3+ criteria | Warning (elevated) |
| Warnings in single criterion | Warning |
| Info only | Info |

### Conflict Resolution

When validation reports conflict:
- Document the contradiction explicitly
- Assess which criterion takes precedence
- Explain rationale for resolution

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Routine review fan-out without a request or risk policy | Consumes resources without changing confidence | Route story completion to the story-completion validator |
| Serializing validators when concurrent dispatch is granted | Inefficient | Launch independent validations in a single message |
| Concurrent dispatch without the `allow-parallel-agents` grant | Conflicting work, lost context inheritance | Run the same validations one at a time |
| Skipping synthesis | Fragmented findings | Always aggregate parallel outputs |
| Fixed five-validator fan-out when one criterion carries the risk | Wasted coverage | Select criteria by risk |
| Returning all report contents | Bloats orchestrator | Return paths, let user read details |
| Ignoring failed validation agents | Incomplete assessment | Document gaps in synthesis |
| Unbounded criteria expansion | Scope creep | Define criteria upfront |

## Relationship to Other Workflows

Review workflow often precedes or follows other workflows:

**Before Implementation**: Review identifies what needs changing
**After Implementation**: Review verifies changes achieved intent — when explicitly requested; routine closure is the story-completion validator's job
**Periodic Assessment**: Review establishes quality baseline
