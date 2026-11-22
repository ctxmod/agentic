# Work Item Writing Workflow

Orchestrate work item creation through discovery, optional analysis, writing, and validation. Handles all work item types (epics, stories, issues, tasks) with identifier reservation and parent context linking.

## Workflow Overview

```
Phase 0: Identifier Reservation (main agent)
         ↓
Phase 1: Context Discovery
         ↓
Phase 2: Analysis (optional, for complex items)
         ↓
Phase 3: Work Item Writing (Opus)
         ↓
      Complete
```

Writers reference objectives, procedures, schemas, and principles during writing, self-verifying against schema requirements. Small body-metadata variations (completed/complete, pending/draft) are acceptable; the authoritative status is the record's state (`ctxmod work show`).

## When to Use

- Creating new epics, stories, issues, or tasks
- Work item requires context from parent or related items
- Identifier reservation required (no explicit ID provided — the record must be created)
- Schema compliance must be verified

For oversized efforts still wrapped in undecided questions, chart a decision map before creating items: `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/decision_map.md`.

## Work Item Types

| Type | Parent | Contains | Location |
|------|--------|----------|----------|
| Epic | None | Stories (by `parent`) | `items/<YYYY-MM>/<DD-HHMM>-<id>-<slug>/` |
| Story (epic-nested) | Epic | Issues (by `parent`) | `items/<YYYY-MM>/<DD-HHMM>-<id>-<slug>/`, `parent=<epic id>` |
| Story (standalone) | None | Issues (by `parent`) | `items/<YYYY-MM>/<DD-HHMM>-<id>-<slug>/` |
| Issue (story) | Story (required) | Tasks (by `parent`) | `items/<YYYY-MM>/<DD-HHMM>-<id>-<slug>/`, `parent=<story id>` |
| Issue (standalone) | None | Tasks (by `parent`) | `items/<YYYY-MM>/<DD-HHMM>-<id>-<slug>/` |
| Task | Issue | None | `items/<YYYY-MM>/<DD-HHMM>-<id>-<slug>/`, `parent=<issue id>` |

Containment is logical: the parent-child link is the record's `parent` field (`ctxmod work set <item> parent=<item>` re-parents without moving a file). Every item lives in its own directory under `items/`; only owned records (reports, intents, artifacts) nest physically, inside their owner's directory. The store enforces the rank order (epic, story, issue, task): a parent ranks before its child, and the parent chain never loops.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## Phase 0: Identifier Reservation

Main agent creates the work item record before launching sub-agents.
`ctxmod work new` is the canonical allocation mechanism; the store is
the SSOT for allocated IDs (each id is minted from a secure random
source and published with an exclusive link, so concurrent sessions
cannot collide). Manual filesystem ID resolution (inspecting
directories and computing the next ID by hand) is collision-prone
under concurrent sessions and is prohibited (`reserve_before_create`
in the ctxmod builtin `work_discipline` profile).

**If identifier provided**: Use directly (e.g., `"Create an issue under story kpqrstbcdf"` → `ctxmod work new issue "<title>" --parent kpqrstbcdf`; `"Write issue mnbvcxzqwr"` → `ISSUE_PATH=mnbvcxzqwr (<its item dir>)`).

**If identifier not provided**: Create the record to obtain the ID and
write-to path:

```bash
ctxmod work new {type} "<title>" [--parent {parent-id}]   # once per item
```

The `{type}` is the work item kind (`epic`, `story`, `issue`, `task`)
and `{parent-id}` is the parent's record id (an epic id for stories
under an epic, a story id for issues under a story, an issue id for
tasks under an issue). Omit the parent for epics and standalone items.
Run the command once per sibling item when several are needed. The
call prints the allocated ID and the `ITEM.md` path; the record exists
from the moment it is created, with no body until the agent writes
`ITEM.md` at the printed path — a record without a body is complete,
so no separate reservation step exists.

## Phase Structure

### Phase 1: Context Discovery

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_general.md" />

**Workflow-specific notes**:
- Discover parent work item context: preferred `ctxmod work show <parent-id>` when parent ID is known; fall back to direct read
- Use the work store for work-item discovery — `work_store_first` in the ctxmod builtin `work_discipline` profile
- Find related work items for linking
- Gather source materials informing the work item

### Phase 2: Analysis (Optional)

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_general.md" />

**When to include**: Complex epics requiring scope decomposition, issues needing problem analysis, research tasks requiring question refinement.

**When to skip**: Simple tasks with clear scope, issues with well-defined requirements, direct user-provided specifications.

### Phase 3: Work Item Writing

the `write_item_phase` skill

**Workflow-specific notes**:
- Always use Opus for writing
- Reference parent work item for context
- Follow schema structure exactly

## Completion

Work items complete after Phase 3. Writers receive objectives, reports, procedures, schemas, and principles when writing, ensuring schema compliance during the writing phase.

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Phase 0: Identifier Reservation (main agent)
- Receives: User request, optional identifier
- Produces: Reserved {TYPE}_ID and write-to path

Phase 1: Context Discovery
- Receives: {TYPE}_PATH, PARENT_PATH
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery.md

Phase 2: Analysis (optional)
- Receives: Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-analysis.md

Phase 3: Work Item Writing
- Receives: {TYPE}_PATH, Phase 1 output, Phase 2 output (if applicable)
- Produces: {TYPE}_PATH/{TYPE}.md
```

Phase report paths are reserved just-in-time by the executing sub-agent (``ctxmod work new report "<title>"``, parent = this work item) — the names above describe the slug convention, not pre-minted dispatch parameters. See params.md's Report Parameters.

## Execution Strategy

### Main Agent Behavior

1. **Reserve identifier**: Call `ctxmod work new` to obtain the work item ID and write-to path
2. **Create directory**: `mkdir -p {resolved path}` (path from Work Item Types table)
3. **Discovery**: Launch discovery sub-agent with parent context
4. **Analysis** (if needed): Launch analysis sub-agent
5. **Writing**: Launch writing sub-agent (Opus) with all context
6. **Complete**: Return work item path

## Workflow Variants

### Lightweight Work Item

Skip analysis for simple items: Identifier Reservation → Discovery → Writing → Validation. Use for tasks with bounded scope, issues with well-defined requirements, or detailed user specifications.

### Epic with Story Decomposition

After epic creation, optionally decompose into stories: Epic Workflow → [Story Workflow × N]. Main agent creates the epic, then launches story workflows for each identified story referencing the parent epic.

### Batch Task Creation

Create multiple tasks for an issue. Create all task records up front
(`ctxmod work new task "<title>" --parent <issue id>`, once per task),
share discovery context across tasks, then write each task sequentially
(Identifier Reservation once, then per task: Writing → Validation).

**Task execution**: Tasks within an issue execute ONE AT A TIME in sequence. Each task receives all previous task reports as compulsory reads before execution. This ensures tasks benefit from previous findings and maintain alignment.

## Worker-Story Execution Loop

Issue-level implementation agents park mid-task with their check gate
running in the background and do **not** reliably wake their parent —
the coordinator, not the worker, owns the resume. The proven loop
(from one story's execution):

1. **Record expected evidence before parking.** After dispatching each
   issue agent, write the expected disk evidence (report path, JSON
   evidence path, planned commit message) into the execution log
   before ending the turn — the resume decision reads this log, not
   the worker's narration.
2. **Resume trust-but-verify.** On coordinator resume: `git show
   --stat` plus an ancestor check plus spot-greps of the load-bearing
   claims; reconcile deviations against the issue's approach notes,
   then dispatch the next issue. The full claim-shape checklist lives
   in `subagent_report_verification` in the ctxmod builtin `delegation` profile.
3. **Compile evidence default: the negative-controlled staticcheck
   floor.** Plant an undefined-symbol scratch file, confirm it is
   reported in BOTH compile units, remove it, require a clean exit 0.
   Under host OOM pressure every scoped check gate may die for
   consecutive issues —
   the gate is the exception, not the rule; do not loop or retry OOM
   kills.

Two execution-round guards ride along: a work-reference pre-commit
stage may veto bare work-item tokens (`S03`-style ids) in committed
comments —
cite inventory rows or prose instead — and story close needs no push
when a peer or automation push already landed the commits (check
`git rev-list --count origin/main..HEAD` before planning one). Commit
shape and push mechanics are owned by
`ctx:github.com/ctxmod/agentic/context::shared::git_mechanics@shared_tree/reference/shared_tree_git_mechanics.md`.

<!-- anchor: worker-story-loop-pattern -->

## Error Handling

| Situation | Response | Recovery |
|-----------|----------|----------|
| Identifier reservation fails | Report the `ctxmod work new` error, await decision | Await user decision |
| Parent not found | Report missing parent, propose orphan or create parent first | Await user decision |
| Validation loop exhausted (3 iterations) | Report persistent schema issues, present current state | Manual correction or accept with known issues |

## Quality Criteria

- **Schema compliance**: Required body sections present, the record's `parent` resolves (`ctxmod work show`), body written at the record's own `ITEM.md`
- **Content quality**: Active voice, concrete language, no AI anti-patterns, appropriate length

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Using Sonnet for writing | Quality degradation | Always use Opus for writing phase |
| Skipping identifier reservation | Path conflicts | Reserve via `ctxmod work new` before launching sub-agents |
| Writing without discovery | Missing context | Gather context first |
| Creating child items in parent workflow | Scope creep | Separate workflow per item |
| Hardcoding identifiers | Collision risk | Reserve via `ctxmod work new` |
