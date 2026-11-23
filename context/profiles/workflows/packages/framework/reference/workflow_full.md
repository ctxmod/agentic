# Full Workflow

Orchestrate autonomous work from initial intent through completed execution. Combines work item creation (Phases 0-6) with execution (Phases 7-17), connected by a handoff gate validating creation output before transitioning to execution context.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Workflow Overview

```
Phase 0: Input Analysis (main agent, direct)
         |
Phase 1a: Organization Analysis ─── OR ─── Phase 1b: Existing Discovery
         |                                            |
Phase 2: Work Decomposition (parallel, conditional)
         |
Phase 3: Creation Planning (fan-in synthesis)
         |
Phase 4: Work Item Creation (Opus, sequential batch execution)
         |
Phase 5: Schema Validation
         |
Phase 6: Creation Remediation Loop (conditional, max 3)
         |
   [Handoff Gate: validate creation, transition context]
         |
Phases 7-17: Execution Workflow (see run.md)
         |
      Complete
```

## When to Use

- Starting from user intent without an existing work item
- Creating and executing in a single autonomous session
- No existing work item structure addresses the requested work
- Parallel creation of related items with immediate execution

For executing an existing work item, use the run workflow. For creation without execution, use the work-item workflow.

## Phase Structure: Creation (Phases 0-6)

### Phase 0: Input Analysis

Main agent performs this phase directly without sub-agents. Analyze the user's description to determine work item type, search for related existing items, decide organization strategy, reserve identifiers via the reserve tools (`reserve_before_create` in the ctxmod builtin `work_discipline` profile), and establish parameters for all subsequent phases.

**Scope assessment**:

| Characteristic | Epic | Story | Issue | Task |
|----------------|------|-------|-------|------|
| Scope | Multiple features | Single feature | Specific work unit | Atomic activity |
| Duration | Spans stories | Deliverable outcome | Trackable independently | Single session |
| Contains | Stories | Issues | Tasks (optional) | Nothing |

**Organization decision**: Create new when no existing parent addresses the work. Append to existing when a parent encompasses the scope and accommodates additional children.

**Output**: Coordination data (`work_type`, `organization_strategy`, `target_parent` if appending, `related_items`) and resolved parameters (`{TYPE}_PATH`, `PARENT_PATH`, `REPORT_BASE`, `TIMESTAMP`).

### Phase 1a: Organization Analysis

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_general.md" />
the `analysis_objective` skill

**Workflow-specific notes**:
- **Conditional**: Executes when `organization_strategy == new` and `work_type` requires decomposition (story or issue level)
- **Mutually exclusive** with Phase 1b
- Sonnet sub-agent analyzes optimal work item organization: story scope, issue decomposition, task breakdown needs, and parallelization opportunities
- Apply sizing guidelines: stories contain 3-10 issues as a practical target

### Phase 1b: Existing Parent Discovery

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_general.md" />

**Workflow-specific notes**:
- **Conditional**: Executes when `organization_strategy == append_existing`
- **Mutually exclusive** with Phase 1a
- Sonnet sub-agent discovers comprehensive context of the existing parent item: scope, child inventory, status, and potential conflicts
- Output includes an append recommendation (confirm append or pivot to new parent)

### Phase 2: Work Decomposition

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

**Workflow-specific notes**:
- **Conditional**: Executes when Phase 1a or 1b identifies items for creation
- **Parallel**: One Sonnet sub-agent per identified item, all launched in a single message
- Each agent analyzes a different item with no shared state: complexity estimation, dependency identification, further decomposition needs
- Fan-in occurs at Phase 3

### Phase 3: Creation Planning

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/general.md`

**Workflow-specific notes**:
- Fan-in synthesis combining Phase 1 organization/discovery output with all Phase 2 decomposition reports
- Produces coordinated creation plan: dependency-resolved sequence, parallelization strategy, identifier reservation scheme
- Sonnet sub-agent; planning does not require Opus

### Phase 4: Work Item Creation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_work_item.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

**Workflow-specific notes**:
- Main agent creates the item records (`ctxmod work new <kind> "<title>" [--parent <id>]`; ids are minted at creation and the store creates the item directories) before launching sub-agents
- **Opus** sub-agents write work items; Sonnet sub-agents validate each item
- **Sequential execution**: Work items execute within batches in sequential order; batches with dependencies execute in dependency order (parent items before children: stories before issues before tasks)
- Each work item receives its own creation-validation-remediation cycle (max 3 iterations per item)

**Context transition**: Once work item directories exist, `REPORT_BASE` updates from the store's top-level `reports/` (no `--owner`) to `<work item dir>/reports/` for all subsequent phases.

### Phase 5: Schema Validation

`ctx:github.com/ctxmod/agentic/context::shared::validate_structure_phase@workflows/reference/structural.md`

**Workflow-specific notes**:
- Cross-work-item validation: record-state completeness, required sections, reference integrity, inter-item consistency
- Sonnet sub-agent validates all created items as a set
- Output: pass/fail with itemized issue list

### Phase 6: Creation Remediation Loop

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />
`ctx:github.com/ctxmod/agentic/context::shared::remediation_phase@workflows/reference/targeted.md`

**Workflow-specific notes**:
- **Conditional**: Triggers when Phase 5 finds issues. **Maximum 3 iterations**.
- Each iteration: correction planning (Sonnet) → correction execution (Opus) → re-validation (Sonnet, Phase 5 checks)
- On exhaustion: Halt, report remaining violations, propose manual correction

## Handoff Gate

Validates creation output before execution begins — the boundary between creation and execution context.

**Validation**: Work item record exists with its body at the resolved path, record state valid and complete (`ctxmod work show`), parent references resolve, all creation reports present. On failure: return to Phase 6 or halt.

**Parameter inheritance**: Execution receives `WORK_ITEM_PATH`, `WORK_ITEM_TYPE`, `PARENT_PATH`, `REPORT_BASE` (now work item directory), `CREATION_REPORTS`, and `TIMESTAMP`.

## Phase Structure: Execution (Phases 7-17)

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_run.md" />

The execution phases follow the run workflow exactly. Phase numbering offset: add 6 to each run workflow phase number (run Phase 1 = full Phase 7, through run Phase 11 = full Phase 17).

**Full-workflow-specific adaptations**:
- Phase 7 inherits creation discovery outputs, providing richer initial context than a standalone run
- Phase 9 may skip when creation discovery is recent and comprehensive
- Report numbering uses `<DD-HHMM>-<id>-10-exec-*` through `<DD-HHMM>-<id>-18-exec-*` prefixes to distinguish execution from creation reports

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Creation (Phases 0-3, REPORT_BASE = the store's top-level `reports/<YYYY-MM>/` (no owner)):
  <DD-HHMM>-<id>-01-organization-analysis.md      Phase 1a (conditional)
  <DD-HHMM>-<id>-01-existing-discovery.md         Phase 1b (conditional)
  <DD-HHMM>-<id>-02-decomposition-{NN}.md         Phase 2 (parallel, conditional)
  <DD-HHMM>-<id>-03-creation-plan.md              Phase 3

Work item (Phases 4-6, REPORT_BASE = <work item dir>/reports/<YYYY-MM>/):
  <DD-HHMM>-<id>-04-creation-*.md                 Phase 4 (per work item)
  <DD-HHMM>-<id>-05-schema-validation.md          Phase 5
  <DD-HHMM>-<id>-06-remediation-{N}.md            Phase 6 (per iteration)

Execution (Phases 7-17):
  <DD-HHMM>-<id>-10-exec-*.md through <DD-HHMM>-<id>-18-exec-*.md (see run.md for report structure)
```

Main agent passes report paths between phases, never report content.

## Execution Strategy

### Main Agent Behavior

1. **Analyze input** (Phase 0, direct): Determine work type, organization strategy, reserve identifiers
2. **Creation phases** (Phases 1-3): Sequential with conditional logic, parallel decomposition in Phase 2
3. **Work item creation** (Phase 4): Launch Opus writers in dependency-ordered batches
4. **Validate, remediate, handoff** (Phases 5-6 + gate): Schema validation, remediation if needed, verify creation before execution
5. **Execute run workflow** (Phases 7-17): Follow run workflow with inherited creation context
6. **Synthesize summary**: Report outcomes, work item path, final status, remaining actions

### Parameter Configuration

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## Conditional Phase Logic

| Phase | Executes When | Skips When |
|-------|---------------|------------|
| 1a | `organization_strategy == new` AND decomposition needed | append_existing or direct single-item |
| 1b | `organization_strategy == append_existing` | new organization |
| 2 | Phase 1 identifies items for creation | No items identified |
| 3 | Phase 2 executed | Phase 2 skipped |
| 4 | Always | Never |
| 5 | Always | Never |
| 6 | Phase 5 validation fails | Validation passes |

Execution conditional logic follows the run workflow.

## Workflow Variants

### Lightweight Full Workflow

Single work item, clear scope, no child decomposition. Phases 1a/1b, 2, and 3 skip; Phase 9 reuses creation discovery. Flow: Input Analysis → Creation Planning → Work Item Creation → Validation → Execution.

### Research-Heavy Full Workflow

Expand Phase 10 with deep research (`perplexity_deep_research`), multiple research rounds, and extended Phase 9 discovery regardless of creation discovery freshness. Use for complex or unfamiliar domains.

### Multi-Artifact Full Workflow

Phase 4 creates multiple work items sequentially; Phases 7-17 execute recursively. Children execute sequentially with each reading previous work item reports, regardless of independence. Parent completes after all children reach terminal state.

## Error Handling

| Situation | Response | Recovery |
|-----------|----------|----------|
| Phase 6 remediation exhausted | Halt, report schema violations | Manual correction, then re-invoke |
| Handoff gate validation fails | Return to Phase 6 or halt | Fix creation output before execution |
| Work item blocked during creation | Halt immediately, no artifacts created | Resolve blocker, retry |
| Identifier reservation fails | Halt, report the `ctxmod work new` error | Await decision / retry |

Execution errors follow the run workflow specification. Key distinction: execution failures preserve all creation output, enabling resumption via `/run {work_item}` without re-running creation.

## Quality Criteria

| Criterion | Validation Method |
|-----------|-------------------|
| Schema compliance | Phase 5 validates record state and required sections |
| Parent reference integrity | Phase 5 verifies parent paths resolve |
| Cross-item consistency | Phase 5 checks dependencies between created items |
| Creation report completeness | Handoff gate validates all phase reports present |

Execution quality criteria follow the run workflow specification.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping Phase 0 identifier reservation | Phase 4 has no item records or directories; collisions under concurrent sessions | Create the item records via `ctxmod work new` first |
| Proceeding to execution with failed handoff | Execution fails on malformed items | Halt at handoff gate, fix creation |
| Duplicating execution phase definitions | Violates SSOT | Reference run workflow for Phases 7-17 |
| Skipping context transition at Phase 4 | Reports scatter across the store | Pass the new work item as `--owner` once its record exists |
| Reading intermediate reports into main agent | Context budget exhaustion | Pass report paths only |
| Concurrent work item creation | Work items are always sequential regardless of posture; each reads the previous items' reports | Create items one at a time with report passing |

## Related Workflows

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_run.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_work_item.md" />
