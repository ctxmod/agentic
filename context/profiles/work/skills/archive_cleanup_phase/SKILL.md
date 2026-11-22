---
description: "Archive completed/stale work items and reports in the ctxmod work store (`ctxmod work archive`). Wraps the CLI with the age-signal hierarchy, the last-touch caveat, and the move-only/preview-first/no-implicit-deletion safety model."
binds_to_depth: phase
---

# Cleanup Work-Archive Phase

Archive completed or stale work items and reports in the ctxmod work
store. Archiving moves a record's body or payload from `<path>` to
`archive/<path>`, keeps its state file, records the instant, and takes
the record out of default listings; archiving an item moves its whole
directory, so everything it owns goes with it. This capability is a
thin wrapper over `ctxmod work list` (selection) and `ctxmod work
archive` (the move) — it surfaces those operations behind a
discoverable command (`/archive_cleanup`) and documents the
age-signal hierarchy, last-touch caveat, and safety model the operator
must understand to use them well.

Work-archive cleanup is a **phase, not a workflow**. The main agent
invokes it directly via the `/archive_cleanup` slash-command.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- The store has accumulated finished items and growth is measurably
  slowing listings (the capability's motivating baseline was a work
  tree of 285 MB, 3,099 items, and 9,025 reports).
- The operator wants to retire completed work to the archive without
  losing it (move-only; nothing is deleted).
- Periodic pruning is warranted — the operator decides cadence; this
  capability runs on manual invocation only.

**When not to use**:

- The concern is test debt — that is the `tests` capability.
- The concern is duplicated production code or compat shims — that is
  the `code-debt` capability.
- The concern is solidifying memories into reference — that is the
  `memory` capability (memory cleanup writes context files, it does
  not archive work items).

## Age-Signal Hierarchy

Age resolves through a hierarchy that is **most-to-least
authoritative** — the first level that yields a date wins, and file
mtime is the fallback of last resort, not the primary signal.

| # | Source | Notes |
|---|--------|-------|
| 1 | An explicit completion field the item carries (an unmanaged `completed_at`, when the workflow sets one) | Exact semantic completion when present. Most precise signal; rare. |
| 2 | The most recent `updated` instant across the item and the records it owns (`ctxmod work list --owner <id>`) | The store stamps `updated` on every change of state and every observed change of a body, so it tracks real work. |
| 3 | Entry-name dates: `<YYYY-MM>/<DD-HHMM>` in the item's and its reports' and intents' entry names, and each record's `created` | Path-derived and immutable: creation dates survive any later reformatting. |
| 4 | File **mtime** | Fallback only, for files outside the store's knowledge. See the caveat below. |

**Why store instants and path dates beat mtime.** Reports and intents
are entries named for their creation instant
(`<owner item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-<slug>.md`,
`…/intents/<YYYY-MM>/…`), and the store records `created` and
`updated` in each state file. Any item that produced a report or
intent carries a durable record of when real work happened — even
after a reformatting sweep resets every mtime. In the legacy corpus the
oldest path-derived date was **two weeks earlier** than the oldest
mtime: mtime was masking real age.

**Filtering rule.** An age threshold of 30 days matches an item when
the **most recent** of its resolved dates (across its own state, its
reports, and its intents) is older than 30 days. "Most recent" avoids
archiving an item that has one stale report and one fresh one.

Legacy measurements behind this ordering (the file-based work tree
the capability was designed on, intent §4 V2): an explicit completion
date covered about 6% of items, report- and intent-directory dates
74%, created/updated fields 6% (all also covered by path dates), and
mtime was the only signal for the 26% residual.

## Last-Touch Caveat

**Last-touch signals reflect touch, not semantic activity.** A
reformatting sweep or a bulk rewrite of item bodies is observed by the
store as body changes and refreshes `updated` (and mtime) across the
tree, making genuinely-old items look new. This biases the selection
toward **under-archiving** — old items with refreshed instants get
skipped.

The bias is the **correct** direction for a destructive operation:
under-archiving leaves an item in place; over-archiving could move
active work. But operators must be aware of it, because an obviously
stale item can be skipped and the reason is not visible without
inspecting the date hierarchy above.

**Practical consequence.** If an item looks stale but the age filter
does not match it, the cause is almost always a recently-refreshed
last-touch signal. Resolve the ambiguity by scoping with an explicit
identifier (`ctxmod work show <id>`, `ctxmod work history <id>`) or by
checking its reports' entry-name dates directly.

## Safety Model

Archive changes what every agent sees, so it is gated by three layers.
The capability inherits these gates verbatim — it must not surface a
shortcut that bypasses them.

| Step | Behavior | Reversibility |
|------|----------|---------------|
| Preview (`ctxmod work list` with the filters, plus the age computation) | Prints the plan; writes nothing | No-op (nothing to reverse) |
| `ctxmod work archive <id>...` | **Moves** each record's body to `archive/<path>` (an item with everything it owns), keeps the state file | Reversible: `ctxmod work restore <id>` |
| Permanent deletion | **No store verb exists** (`work delete` is designed, not built). Never delete store files by hand — the state files would no longer match (`ctxmod work vet` flags it) | Irreversible — out of scope |

**Deletion is not a no-op.** In the legacy corpus a 90-day threshold
matched **83 work items and 509 reports**; permanent deletion would
have destroyed real data. Any deletion surface must therefore be
explicit and two-field (a flag plus a confirmation), never implicit —
and this capability never surfaces a destructive shortcut.

**Default posture.** Use move-only archiving for routine sweeps.
Always preview first; archive only when the plan is reviewed. An
archived record is read-only until restored (every change refuses with
`CTX-WRK013`).

## Underlying Tools

This capability does **not** re-implement archival. It delegates to
the work store's verbs. Reference their docs, do not copy them.

```
ctxmod work list --kind epic,story,issue,task --status done        # candidates (completed)
ctxmod work list --kind report --owner <id>                         # an item's reports
ctxmod work show <id>                                               # dates and state of one record
ctxmod work archive <id>...                                         # move-only archive
ctxmod work list --archive-state archived                           # verify what is archived
ctxmod work restore <id>...                                         # undo
```

Filters compose: `--kind`, `--status`, `--owner`, `--parent`, and
`--archive-state` scope the listing (see `ctxmod work list --help` for
the authoritative flag list). Archived records disappear from default
listings (`--archive-state active` is the default) and reappear with
`--archive-state archived` or `all`.

## Workflow

Three steps, sequential. The operator drives each step; the capability
does not auto-advance.

1. **Preview.** List the candidates with the operator's filters and
   compute each one's resolved age (hierarchy above). Present the plan:
   how many items, how many reports, which identifiers. The preview
   writes nothing — it is the safe step.
2. **Move-only archive.** If the plan is acceptable, run `ctxmod work
   archive <id>...` for exactly the planned records. Nothing is
   deleted. Verify with `ctxmod work list` (archived items no longer
   appear) and `ctxmod work list --archive-state archived`.
3. **(Optional) Retire old archive content.** There is no store verb
   for permanent deletion; if the archive itself has grown too large,
   surface that to the operator as a decision — the capability never
   deletes and never suggests hand deletion.

**Scope narrowing.** Pass identifiers to scope single work items, or
archive only report records (the work item itself stays) by selecting
`--kind report`. Use `--status done` to restrict to finished work;
leave the status filter off to catch items abandoned without a status
flip.

## Manual Invocation Only

This capability is manual-invocation only (intent §3 D2, §5 scope,
§7 AC item 26). **No hook, no CI, no schedule.** The operator runs
`/archive_cleanup` explicitly when store growth warrants a sweep.
Archival is reversible (restore), but it changes every default listing
— automated runs without a human reviewing the preview risk sweeping
active work into the archive.

The capability must not be wired into any automatic check or
background process. The operator must see the plan before it executes.

## Error Handling

| Situation | Response |
|-----------|----------|
| Preview finds zero matches | Report and exit (nothing to archive) |
| A deletion is requested | Decline; no store verb exists; surface as an operator decision |
| An item matches but has no reports and no intents | Its own `updated`/`created` apply; document the last-touch caveat to the operator |
| Archive fails (lock, disk, permissions) | Stop, surface the error, leave partial state for manual inspection (`ctxmod work vet`) |
| Identifier scope resolves to nothing | Report "no such item" (`CTX-WRK004`) and exit |

Error handling follows stop-report-propose-await pattern.

## Quality Criteria

| Criterion | Validation Method |
|-----------|-------------------|
| Age hierarchy documented | Body explains store-instant and path-date primacy and the four-level table |
| Last-touch caveat documented | Body states last-touch signals reflect touch, not semantic activity |
| Safety model documented | Body explains preview-first, move-only archive, and the absence of implicit deletion |
| Reference over duplication | Body links to the `ctxmod work` verbs rather than copying their docs |
| Manual invocation only | No hook, CI, or schedule wiring anywhere in the body |
| Deletion never implicit | No code path or suggested invocation reaches deletion |

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Archiving without a preview first | Always preview the plan |
| Deleting store files by hand | Archive (reversible); surface deletion as an operator decision |
| Inspecting `git status` to verify archival | The store lives outside the committed tree — inspect `ctxmod work list --archive-state archived` |
| Re-documenting the CLI flags in the skill body | Link to `ctxmod work list --help` / `ctxmod work archive --help` |
| Auto-advancing from preview to archive | The operator decides each step; the capability does not chain |
| Trusting mtime as the primary age signal | Use store instants and path dates; mtime is the fallback |

## Sub-Layer Launch

This phase has no objective or procedure sub-layer. The capability is
a single phase that delegates to the `ctxmod work` verbs — there is no
analysis objective to dispatch and no multi-step procedure to extract.
The work happens in the tool, not in a dispatched agent.

If a future caller wants the capability invoked from a workflow,
launch the `archive_cleanup_phase` agent directly:

Brief template for the `archive_cleanup_phase` agent — Execute the work-archive cleanup phase:

```text
<ctxmod_activate skill="archive_cleanup_phase" />
Preview the sweep with the operator's filters, present the plan,
await confirmation, then execute the move-only archive. Never delete.
```

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile
