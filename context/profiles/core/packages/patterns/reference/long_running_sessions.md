# Long-Running Sessions

Work that spans many context windows or many sessions fails in two
characteristic ways: the agent tries to do everything at once and runs
out of context mid-change, or a later session sees progress and
declares the whole task done. The structure below counters both. It is
a pattern for one agent working across sessions; it composes with the
multi-agent patterns of this package rather than replacing them.

## Session roles

- **Initializer session** (first run only): turns the request into an
  explicit, checkable work list — every item initially failing — plus
  a script that starts the environment, an empty progress log, and an
  initial commit. The work list is the definition of done that later
  sessions cannot quietly shrink.
- **Worker sessions** (every later run): orient, take **one** item,
  finish it, verify it end to end, and leave clean state. The two roles
  can share one harness and differ only in their opening brief.

In this module the work store plays the work-list role: work items with
acceptance criteria are the checkable list, and reports are the
progress log (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />).

## Worker session loop

1. **Orient from disk, not memory**: read the progress log and recent
   commits, run the start script, and run a quick smoke check of what
   already works before touching anything — a broken baseline found
   now is cheaper than one found after new edits land on it.
2. **Take one item.** Incremental scope is what keeps a session inside
   its window and makes each change reviewable.
3. **Verify as a user would.** Unit checks and a single request against
   an endpoint routinely pass while the feature is broken end to end;
   drive the real surface before marking the item passing
   (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/e2e_validation.md" />).
4. **Leave clean state**: code fit to merge, a descriptive commit, and a
   progress entry naming what changed and the exact resume point.

## Artifact rules

- **The work list only gains status.** Workers flip an item's status;
  they never delete or reword items or their checks, because a removed
  check is a silently dropped requirement. Structured data (JSON, a
  status field per item) resists that rewriting better than prose.
- **Version control is the undo log.** Descriptive commits per item let
  a later session revert a bad change instead of debugging it.
- **The durable record outlives the window.** Progress lives in files,
  commits, and the work store — never only in the context window, which
  compaction and resets rewrite irreversibly. The context-side
  techniques (compaction, clearing, notes) are
  `ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/long_horizon_context.md`.

## Compaction or reset

Compaction keeps one agent going on a summary of its own history; a
**reset** starts a fresh agent from a structured hand-off file. Resets
cost orchestration, tokens, and latency, and earn them only when a
model degrades near its context limit — one model generation wrapped
work up prematurely as the window filled, and resets cured it; the next
generation lost the behavior and the resets became dead weight. Choose
per model, by measurement, and revisit the choice when the model
changes.

## Separate the judge

Agents grade their own work leniently, most of all on subjective
quality. Where a step's quality is beyond what the model reliably
achieves alone, a separate evaluator with explicit, thresholded
criteria — agreed with the generator before the work starts — catches
what self-review approves. Its value is model- and task-relative: when
the model handles the task reliably solo, a final single pass or none
is enough. Deterministic oracles still come first
(<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />).

## Sources

- https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents — initializer and coding-agent sessions, JSON feature list all failing at start and edited only in status, progress file, `init.sh`, one feature per session, clean merge-ready state, browser-driven end-to-end checks (Anthropic Engineering, 2025-11-26; accessed 2026-11-22)
- https://www.anthropic.com/engineering/harness-design-long-running-apps — context anxiety, compaction versus resets, planner/generator/evaluator, self-evaluation leniency, pre-agreed sprint contracts, evaluator value as task-relative (Anthropic Engineering, 2026-03-24; accessed 2026-11-22)
- https://www.anthropic.com/engineering/managed-agents — durable session log outside the context window; resets becoming "dead weight" on a newer model (Anthropic Engineering, 2026-04-08; accessed 2026-11-22)
