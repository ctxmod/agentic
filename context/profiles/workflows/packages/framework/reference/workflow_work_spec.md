# Spec Workflow

Orchestrate specification validation, research, work item creation, and correction. Spec workflow evaluates specifications against multiple criteria, researches questionable approaches, creates issues for findings, and applies corrections through iterative verification.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Workflow Overview

Spec workflow uses parallel validation and research with conditional phases:

```
Phase 0: Spec Discovery (direct)
         ↓
Phase 1: Multi-Criteria Validation (Parallel)
         ├── Structural validation
         ├── Standards validation
         ├── Writing validation
         └── Maintenance validation
         ↓
Phase 2: Finding Analysis
         ↓
Phase 3: Questionable Approach Research (Parallel, conditional)
         ├── Web research
         ├── Academic research
         └── Standards research
         ↓
Phase 4: Research Synthesis (conditional)
         ↓
Phase 5: Work Item Planning
         ↓
Phase 6: Work Item Creation (sequential, conditional)
         ├── Story creation (if new)
         └── Issue creation (sequential)
         ↓
Phase 7: Correction Application
         ↓
Phase 8: Verification Loop (max 3 iterations)
         ↓
      Complete
```

## When to Use

Use spec workflow when:

- Technical specifications require comprehensive validation
- Questionable approaches benefit from external research
- Findings organize into story-based work items
- Corrections need iterative verification

Spec workflow validates specifications through multi-criteria assessment with research-backed corrections.

## Phase Structure

### Phase 0: Spec Discovery (direct)

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_general.md" />

**Workflow-specific notes**:
- Main agent performs directly (no sub-agent)
- Catalog spec structure and sections
- Extract frontmatter and characteristics
- Output: discovery report

### Phase 1: Multi-Criteria Validation (Parallel)

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_general.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

**Workflow-specific notes**:
- Parallel pattern: launch 4 validation agents
- Dimensions: structural, standards, writing, maintenance
- Each agent validates single dimension
- Fan-in at Phase 2

### Phase 2: Finding Analysis

the `analysis_objective` skill

**Workflow-specific notes**:
- Sequential pattern aggregating all validations
- Extract research questions
- Classify findings by severity
- Identify auto-fixable issues

### Phase 3: Questionable Approach Research (Parallel, conditional)

**Conditional execution**: Skip when Phase 2 reports no research questions.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_general.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

**Workflow-specific notes**:
- Parallel pattern: launch 3 research agents
- Sources: web, academic, standards
- Each agent researches from single source type
- Fan-in at Phase 4

### Phase 4: Research Synthesis (conditional)

**Conditional execution**: Skip when Phase 3 was skipped.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_synthesis.md" />

**Workflow-specific notes**:
- Sequential pattern aggregating all research
- Resolve contradictions with rationale
- Map findings to original questions
- State confidence levels

### Phase 5: Work Item Planning

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/work_item.md`

**Workflow-specific notes**:
- Sequential pattern for planning
- Main agent resolves identifiers and story context
- Group findings into issues
- Identify auto-fixable vs complex issues
- Determine parallelization strategy

### Phase 6: Work Item Creation (Sequential)

**Conditional execution**: Skip when Phase 5 reports no issues.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_work_item.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

**Workflow-specific notes**:
- Sequential story creation (if new), then sequential issue creation. Each issue receives all previous work item reports as compulsory reads.
- Create story with proper parent reference
- Launch agents for issues sequentially, one at a time
- Issues reference spec in `related` field

### Phase 7: Correction Application

`ctx:github.com/ctxmod/agentic/context::shared::remediation_phase@workflows/reference/general.md`

**Workflow-specific notes**:
- Sequential pattern for corrections
- Apply auto-fixable and research-dependent corrections
- Organize by category to minimize disruption
- Track all modifications

### Phase 8: Verification Loop

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />

**Workflow-specific notes**:
- Validation loop pattern with max 3 iterations
- Re-run all 4 Phase 1 validation criteria per iteration
- Remediation agent addresses remaining issues
- Exit on success (zero errors, warnings < 5) or iteration exhaustion

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Phase 0: reports/<YYYY-MM>/<DD-HHMM>-<id>-01-discovery.md
Phase 1: reports/<YYYY-MM>/<DD-HHMM>-<id>-02-val-{dimension}.md (parallel)
Phase 2: reports/<YYYY-MM>/<DD-HHMM>-<id>-03-analysis.md
Phase 3: reports/<YYYY-MM>/<DD-HHMM>-<id>-04-research-{source}.md (parallel, conditional)
Phase 4: reports/<YYYY-MM>/<DD-HHMM>-<id>-05-synthesis.md (conditional)
Phase 5: reports/<YYYY-MM>/<DD-HHMM>-<id>-06-planning.md
Phase 6: {STORY_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-07-creation.md (conditional)
         {ISSUE_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-07-creation.md (one per issue, conditional)
Phase 7: reports/<YYYY-MM>/<DD-HHMM>-<id>-08-corrections.md
Phase 8: reports/<YYYY-MM>/<DD-HHMM>-<id>-09-iter-{N}-validation.md (loop)
         reports/<YYYY-MM>/<DD-HHMM>-<id>-09-iter-{N}-remediation.md (loop)
```

## Variants

### No Research Needed

When no questionable approaches:

```
Discovery → Validation → Analysis → Planning → Creation → Corrections → Verification
```

Skip Phases 3-4.

### No Issues Found

When validation passes initially:

```
Discovery → Validation → Analysis → Complete
```

Skip Phases 3-7.

### Auto-Fix Only

When all issues are auto-fixable:

```
Discovery → Validation → Analysis → Planning → Corrections → Verification
```

Skip work item creation.

### Research-Heavy

When many questionable approaches:

```
Discovery → Validation → Analysis → Research (3 parallel) → Synthesis → Planning → ...
```

Full research workflow.

## Error Handling

| Situation | Response |
|-----------|----------|
| Spec file not found | Halt, report path error |
| No research questions | Skip Phases 3-4 |
| No issues found | Skip Phases 6-7, complete |
| Verification loop exhausted | Report remaining issues, propose manual intervention |

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Researching without questionable approaches | Skip research when no questions |
| Creating issues for auto-fixable problems | Apply corrections directly |
| Single validation pass | Use verification loop for quality convergence |
| Skipping final verification | Always verify corrections meet criteria |
