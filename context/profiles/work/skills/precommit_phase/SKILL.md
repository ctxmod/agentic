---
description: "Run precommit validation, create or append to story with issues, execute to completion."
binds_to_depth: phase
---

# Work Precommit Phase

Validate all repository files against non-test precommit hooks, create
work items for any issues discovered, and execute those work items to
completion. Bridges precommit validation with story-based work item
execution.

Validation covers non-test checks only — formatting, lint, reference
validity, and commit safety. No phase here executes tests:
the automatic legacy test-suite invocation is removed, and test
evidence enters only through deliberate selection — deliberately
selected package tests or a deliberately invoked test tool (live
capability) — never as an automatic part of discovery or final validation.

Work precommit is a **phase, not a workflow**. The main agent invokes
it directly via the `/precommit` slash-command.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/patterns_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflows_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_work_precommit.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_run.md" />

## When to Use

Use this phase when:

- Precommit hooks surface issues that need tracked remediation
- Issues should organize into a story (new or existing) for coherent execution
- Final validation must confirm all issues are resolved

**When not to use**:

- A single specific issue is known — use `/implement_fix` directly.
- Precommit passes cleanly — nothing to do.

## Phase Orchestration

Four phases, conditional. The work/precommit workflow reference
carries the per-phase shape; the run workflow reference drives the
execution phase.

```
Phase 1: Precommit Discovery
         ↓  (run non-test hooks, classify failures, apply auto-fixes)
Phase 2: Work Item Creation (conditional)
         ↓  (only if manual-fix issues remain)
Phase 3: Execution Orchestration
         ↓  (execute work items via run workflow)
Phase 4: Final Validation
         ↓
      Complete
```

**Completion checks**: executed work items close on scoped
deterministic checks, not on a test-suite pass. Deliberately selected
package tests or a deliberate test-tool invocation (live capability)
is the
only path to test evidence, chosen explicitly per work item — never
run automatically by this phase.

**Story creation behavior** (main agent decides before Phase 2):

| User Input | Story Creation | Issue Placement |
|------------|----------------|-----------------|
| `--story {id}` provided | Use existing story | Append issues to existing story |
| `--epic {id}` provided | Create new story under epic | Create issues under new story |
| Neither provided | Create standalone story "Precommit Fixes" | Create issues under story |

**Identifier reservation** (main agent performs before launching
sub-agents):

The main agent calls `ctxmod work new` to obtain collision-safe IDs
before creating any work item. Manual filesystem ID resolution is
prohibited.

- Story ID: `ctxmod work new story "<title>" --parent <epic id>`
- Issue IDs: `ctxmod work new issue "<title>" --parent <story id>` (once per issue)

The records exist from creation but carry no body yet — write each work
item's `ITEM.md` at the printed path.

**Conditional phase execution**:

| Phase | Executes When | Skips When |
|-------|---------------|------------|
| 1 | Always | Never |
| 2 | Precommit found manual-fix issues | No issues or all auto-fixed |
| 3 | Phase 2 created work items | Phase 2 skipped |
| 4 | Phase 3 executed | Phase 3 skipped |

## Work Item Organization

Group precommit findings into story and issues using these strategies:

| Finding Pattern | Issue Strategy | Rationale |
|----------------|----------------|-----------|
| Same file, same issue type | Single issue, possibly with tasks | Coherent fix scope |
| Multiple files, same issue type | Single issue | Consistent fix pattern |
| Different issue types | Separate issues | Independent fixes |
| Security issues | Separate issue, high priority | Safety-critical |

**Story organization**:

- All issues created under a single story (new or existing)
- Story `parent` field references epic if provided, otherwise `null`
- Issue `parent` field always references story identifier
- Issues may decompose into tasks for complex fixes

## Information Flow

```
Phase 1: Discovery
- Receives: --staged-only flag
- Produces: precommit findings (auto-fixed + manual-fix)

Phase 2: Work Item Creation (conditional)
- Receives: Phase 1 manual-fix findings, --epic/--story flags
- Produces: story + issues (or appends to existing story)

Phase 3: Execution
- Receives: Phase 2 work items
- Produces: executed changes per run workflow

Phase 4: Final Validation
- Receives: Phase 3 execution result
- Produces: validation verdict
```

Main agent passes report **paths** between phases, never report content.

## Dispatch Strategy

Every phase here is sequential. This phase agent is not granted
`allow-parallel-agents: true`, and Phase 3 executes **work items**,
which are always sequential regardless of the posture: each work item
reads all previous work item reports before executing.

| Phase | Dispatch | Rationale |
|-------|----------|-----------|
| 2 | Sequential (single plan) | N/A |
| 3 | Sequential, one work item at a time | Work items produce context that informs the next |
| Phase 3 per-workitem | Follows run workflow | Per work item type |

## Error Handling

| Situation | Response |
|-----------|----------|
| No precommit configuration | Report and exit (nothing to do) |
| All issues auto-fixed | Report and exit (no work items needed) |
| Work item validation fails | Follow run workflow remediation |
| Final validation fails | Report remaining issues, propose manual intervention |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Creating work items when auto-fix suffices | Skip Phase 2 when no manual-fix issues |
| Scattering issues across stories | Group all precommit issues under one story |
| Skipping final validation | Always verify precommit passes after execution |
| Running a test suite during discovery or final validation | Tests never execute automatically; deliberate `--work` (landed) or `test:` (landed) selection only |

## Sub-Layer Launch

When this phase requires execution-depth work, launch the `workflow`
agent for the run-workflow phases:

Brief template for a workflow-depth agent — Execute precommit-discovered work items:

```text
Execute the run workflow for the precommit-discovered work items.
Story: <story-id>
Issues: <issue-id list>
```

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"validate and fix precommit issues" as a sub-step. Launch the
`precommit_phase` agent from any workflow that needs precommit
remediation.

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile
`reserve_before_create` in the ctxmod builtin `work_discipline` profile
