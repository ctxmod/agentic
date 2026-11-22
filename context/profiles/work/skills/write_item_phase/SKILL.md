---
description: "Work item writing phase — produce tracked work units (epic, story, issue, task variants) through discovery, planning, and writing with conditional validation."
binds_to_depth: phase
---

# Write Work Item Phase

Work item writing phase — produce tracked work units in the work item
hierarchy (epics, stories, issues, tasks). This is one of four sibling
phase skills in the writing capability (report, work-item, context,
intent); they share the same methodology layer (`writing_phase`) and
the shared objective (`writing_objective`) and procedure
(`writing_procedure`).

Work item writing is a **phase, not a workflow**. The main agent
invokes it directly via the absorbed `/write-epic`, `/write-story`,
`/write-issue`, `/write-task` slash-commands, the `/write-work` router,
or any other domain's workflow agent references this phase as a "write
work item" sub-step.

## Absorbed commands

Four work-item variants absorb into this single phase skill. The
variant is determined by the artifact type argument.

### `write-epic` (epic variant)

```yaml
name: write-epic
key: wri-epic
invocation: user
summary: Create strategic initiative defining major multi-story scope
usage: /write-epic [description]
work_item_context: none
```

`/write-epic [description]` (original form: `/write:work:epic [description]`)

| Argument | Required | Description |
|----------|----------|-------------|
| `description` | Yes | Strategic initiative description |

Output: the epic's `ITEM.md` (`ctxmod work new epic "<title>"` prints it). Epics have no parent context.

### `write-story` (story variant)

```yaml
name: write-story
key: wri-story
invocation: user
summary: Create feature specification defining deliverable outcomes
usage: /write-story [description] [--epic <epic id>]
work_item_context: optional
```

`/write-story [description] [--epic <epic id>]` (original form: `/write:work:story [description]`)

| Argument | Required | Description |
|----------|----------|-------------|
| `description` | Yes | Feature description |
| `--epic` | No | Parent epic identifier |

Output: the story's `ITEM.md` — created standalone, or with
`--parent <epic id>` (epic-nested).

### `write-issue` (issue variant)

```yaml
name: write-issue
key: wri-issue
invocation: user
summary: Create work unit description with completion criteria
usage: /write-issue [description] [--story <story id>]
work_item_context: optional
```

`/write-issue [description] [--story <story id>]` (original form: `/write:work:issue [description]`)

| Argument | Required | Description |
|----------|----------|-------------|
| `description` | Yes | Issue description |
| `--story` | No | Parent story identifier |

Output: the issue's `ITEM.md` — created with `--parent <story id>`
(story-scoped) or standalone.

### `write-task` (task variant)

```yaml
name: write-task
key: wri-task
invocation: user
summary: Create granular activity within an issue
usage: /write-task [description] --issue <issue id>
work_item_context: required
```

`/write-task [description] --issue <issue id>` (original form: `/write:work:task [description] --issue I{NN}]`)

| Argument | Required | Description |
|----------|----------|-------------|
| `description` | Yes | Task description |
| `--issue` | Yes | Parent issue identifier |

Output: the task's `ITEM.md`, created with `--parent <issue id>`.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Creating tracked work units in the work item hierarchy
- Work item schema compliance matters
- Parent-child relationships need correct resolution
- Identifier reservation and directory creation are required

## Variant Dispatch

The variant determines identifier reservation, parent context, and
output location. All variants share the same phase structure below;
variant-specific differences are noted per phase.

| Variant | Parent context | Reserve tool |
|---------|----------------|-------------|
| `epic` | None (creates context) | `ctxmod work new epic "<title>"` |
| `story` | Optional epic | `ctxmod work new story "<title>" --parent <epic id>` |
| `issue` | Optional story | `ctxmod work new issue "<title>" --parent <story id>` |
| `task` | Required issue | `ctxmod work new task "<title>" --parent <issue id>` |

## Phase Orchestration

This phase dispatches objective-depth sub-agents through the shared
`writing_objective`, with work-item-specific reference material below
scoping each dispatch. The `writing_phase` methodology layer carries
the variant-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns).

Phase 0 reserves the work-item ID itself; phase report paths are
different — they are reserved by the executing sub-agent just-in-time
(`ctxmod work new report "<phase title>"`(
parent=<reserved work item ID>)`) immediately before writing. The
`- Report:` lines below name the slug convention each reservation uses;
they are never pre-minted dispatch parameters. Sub-agents return their
reserved paths, and the main agent passes those returned paths onward,
never report content.

```
Phase 0: Identifier Reservation (main agent)
         |
Phase 1: Context Discovery (Sonnet)
         |
Phase 2: Analysis (optional, for complex items)
         |
Phase 3: Writing (Opus)
         |
Phase 4: Validation (conditional)
```

### Phase 0: Identifier Reservation (main agent)

Main agent calls `ctxmod work new` to obtain the ID and the write-to
path before launching sub-agents. No sub-agent needed. The record is
published in the store with an exclusive link, which prevents
concurrent sessions from colliding on the same ID. Manual filesystem
ID scans are prohibited — `ctxmod work new` is the only collision-safe
mechanism.

**Canonical invocation**:

```bash
ctxmod work new story "<title>" --parent <epic id>
```

**Variant-specific reservation**:

| Variant | Reservation call |
|---------|-----------------|
| epic | `ctxmod work new epic "<title>"` |
| story (standalone) | `ctxmod work new story "<title>"` |
| story (epic-nested) | `ctxmod work new story "<title>" --parent <epic id>` |
| issue (story-scoped) | `ctxmod work new issue "<title>" --parent <story id>` (once per item) |
| issue (standalone) | `ctxmod work new issue "<title>"` |
| task | `ctxmod work new task "<title>" --parent <issue id>` |

The call returns the new ID and the `ITEM.md` path to write. The store
creates the item directory; the agent still writes the work-item body
itself.

### Phase 1: Context Discovery

Gather context for work item creation.

- Report: `<work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-01-discovery.md`

**Workflow-specific notes**:
- Discovery only; do not write work item
- Gather parent context if provided
- Identify related items via the related list in item bodies and the `parent` field (`ctxmod work list --parent <id>`)
- For epics: discover related strategic initiatives
- For stories: discover parent epic scope
- For issues: discover parent story acceptance criteria
- For tasks: discover parent issue completion criteria

### Phase 2: Analysis (optional)

Analyze scope and decomposition for complex items. Skip for atomic
tasks.

- Report: `<work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-02-analysis.md`

**Workflow-specific notes**:
- Analysis only; do not write work item
- For epics: analyze story decomposition
- For stories: analyze issue decomposition
- For issues: analyze task decomposition
- Tasks typically skip analysis (atomic)

### Phase 3: Writing (Opus)

Produce the work item document.

- Output: `<work item dir>/ITEM.md` for an epic, story, issue, or task

**Workflow-specific notes**:
- Write work item only
- Follow schema structure exactly
- Record fields complete (kind, status, title, parent if applicable, and the unmanaged fields such as priority set with `--set`)
- Reference parent context in the body's related list
- Reference intent files if work was informed by intent

### Phase 4: Validation (Conditional)

**When to validate**:
- Schema-validated work items
- Cross-reference integrity critical

**When to skip**:
- Atomic tasks (overhead exceeds value)

If validating, apply the validation loop (max 3 remediation iterations).

## Information Flow

```
Phase 0: Identifier Reservation
- Receives: variant, parent context
- Produces: WORK_ITEM_PATH, WORK_ITEM_ID

Phase 1: Context Discovery
- Receives: WORK_ITEM_PATH, parent context paths
- Produces: <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-01-discovery.md

Phase 2: Analysis (optional)
- Receives: Phase 1 output
- Produces: <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-02-analysis.md

Phase 3: Writing
- Receives: WORK_ITEM_PATH, Phase 1 output, Phase 2 output (if run)
- Produces: {WORK_ITEM_PATH}/{TYPE}.md

Phase 4: Validation (conditional)
- Receives: Work item path
- Produces: <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-03-validation.md
```

Main agent passes report **paths** between phases, never report content. Each Produces path is reserved by the producing sub-agent just-in-time and reaches the next phase via the return message, not the dispatch.

## Execution Strategy

1. **Resolve variant**: Determine epic/story/issue/task from arguments
2. **Identifier Reservation**: Main agent calls `ctxmod work new`, obtains ID and path
3. **Discovery**: Launch discovery sub-agent; wait
4. **Analysis** (optional): Launch analysis sub-agent for complex items
5. **Writing**: Launch writing sub-agent (Opus); wait
6. **Conditional Validation**: Launch validation if schema-critical
7. **Complete**: Return work item path and validation summary

## Workflow Variants

### Direct Epic Creation

Phase 0 determines `variant` is epic. Skip Phase 2 (no decomposition
needed at epic level). Simplified Phase 3 (single epic).

### Batch Issue Creation

For multiple issues within a story, reserve all issue IDs in one call,
share discovery context, then write each issue sequentially:
1. Reserve issue IDs: `ctxmod work new issue "<title>" --parent <story id>` (once per item)
2. Run Phase 1 once for the parent story
3. For each issue, run Phase 3 (writing)

### Lightweight Task

Phase 2 skipped (atomic tasks). Simplified Phase 3 (single task).

### Story Append

When appending to existing story, Phase 1 discovers existing story
context; Phase 3 writes new issue referencing parent story.

## Error Handling

| Situation | Response |
|-----------|----------|
| Identifier reservation fails | Report reservation error, await decision |
| Parent context missing (required) | Report missing parent, await decision |
| Validation exceeds iterations | Report persistent issues, await decision |
| Writing fails | Check planning output, await decision |

Error handling follows stop-report-propose-await pattern.

## Quality Criteria

| Criterion | Validation Method |
|-----------|-------------------|
| Record fields complete | Phase 4 checks required fields (`ctxmod work show`) |
| Schema compliance | Phase 4 verifies section presence |
| Parent reference resolves | Phase 4 validates the record's `parent` and the body's related list |
| Acceptance criteria testable | Phase 4 checks for vague language |

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Writing without discovery | Uninformed item | Discover parent context first |
| Expanding scope beyond type | Mixed concerns | Write one item type per phase |
| Skipping identifier reservation | Collisions | Always reserve ID first via `ctxmod work new` |
| Returning item content | Bloats orchestrator | Return path only |
| Omitting parent reference | Hierarchy breaks | Always create the item with `--parent` (or `ctxmod work set <id> parent=<id>`) |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`writing_objective` agent. The objective in turn launches
`writing_procedure` for step mechanics.

Brief template for the `writing_objective` agent — Execute <phase-name> within work item writing:

```text
<ctxmod_activate skill="agentic_workflows_writing_objective" />
Artifact category: work-item
Variant: <epic|story|issue|task>
Phase-specific instructions:
  <phase-specific parameters>
Work-item files: write to {OUTPUT_PATH}. Reports: reserve the path just-in-time — `ctxmod work new report "<title>" --owner <reserved work item ID>` immediately before writing; write there and return the path.
```

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"write work item" as a sub-step. Launch the `write_item_phase`
agent from any workflow that needs work item creation.

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every writing phase
variant:

the `writing_phase` skill

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile
`reserve_before_create` in the ctxmod builtin `work_discipline` profile

Depth-scoped writing guidance for the sub-agents this phase launches:

the `writing_objective` skill
the `writing_procedure` skill

## Sibling Phase Reference

The other two writing capability phase skills:

the `write_report_phase` skill
the `write_context_phase` skill (`github.com/ctxmod/writing_context/context`, `authoring` profile)

Intent writing moved to its own `intent` domain:

the `intent_write` skill
