---
description: "Validate technical specifications, research questionable approaches, create story with issues, apply corrections."
binds_to_depth: phase
---

# Work Spec Phase

Validate technical specifications against structural, normative,
writing, and maintenance criteria. Research questionable approaches
through external sources. Create issues for findings. Apply
corrections. Verify quality through iterative validation loop.

Work spec is a **phase, not a workflow**. The main agent invokes it
directly via the `/spec` slash-command.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/patterns_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflows_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_work_spec.md" />

## When to Use

Use this phase when:

- A specification requires validation against multiple criteria
- Questionable approaches need external research before approval
- Findings should organize into story-based work items
- Corrections must be applied and verified iteratively

## Phase Orchestration

Nine phases, conditional. The work/spec workflow reference carries
the per-phase shape.

```
Phase 0: Spec Discovery (direct, main agent)
         ↓
Phase 1: Multi-Criteria Validation (4 dispatches)
         ↓  (4 validators: structural, standards, writing, maintenance)
Phase 2: Finding Analysis
         ↓  (aggregate, extract research questions)
Phase 3: Questionable Approach Research (3 dispatches, conditional)
         ↓  (3 research agents: web, academic, standards)
Phase 4: Research Synthesis (conditional)
         ↓
Phase 5: Work Item Planning
         ↓
Phase 6: Work Item Creation (sequential, conditional)
         ↓
Phase 7: Correction Application
         ↓
Phase 8: Verification Loop (max 3 iterations)
         ↓
      Complete
```

**Story creation behavior** (main agent decides before Phase 5):

| User Input | Story Creation | Issue Placement |
|------------|----------------|-----------------|
| `--story {id}` provided | Use existing story | Append issues to existing story |
| `--epic {id}` provided | Create new story under epic | Create issues under new story |
| Neither provided | Prompt user for decision | Based on user choice |

**Prompt for missing context** (main agent, when neither flag provided):

```
No story or epic specified. How should issues be organized?

1. Create new epic + story (e.g., "Specification Quality Improvement")
2. Create standalone story (e.g., "Fix {spec-name} Issues")
3. Cancel (use --story or --epic flag to specify)

Enter choice (1/2/3):
```

**Identifier reservation** (main agent performs before launching
sub-agents):

The main agent calls `ctxmod work new` to obtain collision-safe IDs
before any work item is created. Manual filesystem ID resolution is
prohibited — concurrent sessions can both compute the same ID and
clobber each other's writes.

- Story ID: `ctxmod work new story "<title>" --parent <epic id>`
- Issue IDs: `ctxmod work new issue "<title>" --parent <story id>` (once per issue)

The records exist from creation but carry no body yet — the main agent
must still write each work item's `ITEM.md` at the printed path.

**Conditional phase execution**:

| Phase | Executes When | Skips When |
|-------|---------------|------------|
| 3-4 | Research questions exist | No research questions |
| 6 | Issues found in planning | No issues found |
| 8 | Issues created or corrections applied | Earlier phases skipped |

## Work Item Context

This phase operates with optional work item context. Specification
validation creates or appends to stories organizing issues
hierarchically.

**Issue organization**:

- Issues always created under stories (never standalone with `parent: null`)
- Single-issue stories acceptable if issue decomposes into tasks
- Story `parent` field references epic or `null` for standalone stories
- Issue `parent` field references story identifier

**Report organization**: Reports nest within story directories when
work item context exists, otherwise in the store's top-level `reports/` (no `--owner`).

## Information Flow

```
Phase 0: Spec Discovery
- Receives: spec-path
- Produces: spec structure catalog

Phase 1: Validation (4 dispatches)
- Receives: Phase 0 catalog
- Produces: per-criterion validation reports

Phase 2: Finding Analysis
- Receives: Phase 1 reports
- Produces: aggregated findings + research questions

Phase 3: Research (3 dispatches, conditional)
- Receives: Phase 2 research questions
- Produces: per-source research reports

Phase 4: Research Synthesis (conditional)
- Receives: Phase 3 reports
- Produces: synthesized research findings

Phase 5: Work Item Planning
- Receives: Phase 2 + Phase 4 outputs
- Produces: story + issue plan

Phase 6: Work Item Creation (conditional)
- Receives: Phase 5 plan
- Produces: created story + issues

Phase 7: Correction Application
- Receives: Phase 6 work items
- Produces: applied corrections to spec

Phase 8: Verification Loop
- Receives: Phase 7 corrections
- Produces: validation verdict (re-validate if fails)
```

Main agent passes report **paths** between phases, never report content.

## Error Handling

| Situation | Response |
|-----------|----------|
| Spec file not found at spec-path | Halt, report missing file |
| No criteria applicable | Report and exit |
| Research yields no relevant results | Proceed with available sources, note gaps |
| Verification loop exceeds 3 iterations | Report unresolved issues, halt |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Skipping multi-criteria validation | Run all four validators, one at a time by default |
| Applying corrections without planning | Always plan work items first |
| Single verification pass | Use the verification loop (max 3) |
| Creating standalone issues without story | Always nest issues under a story |
| Creating work items concurrently | Work items are always sequential; each reads the previous items' reports |

## Sub-Layer Launch

When this phase requires validation-depth or research-depth
execution, launch the `workflow` agent for the respective phases:

Brief template for a workflow-depth agent — Validate spec against criteria:

```text
Execute the validation phase for the spec at <spec-path>.
Criterion: <structural|standards|writing|maintenance>
```

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"validate a spec" as a sub-step. Launch the `spec_phase` agent
from any workflow that needs specification validation.

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile
`reserve_before_create` in the ctxmod builtin `work_discipline` profile
