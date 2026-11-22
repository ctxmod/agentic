---
description: "Analyze the work store's memory records, solidify durable knowledge into the repository's context-module reference via the existing context-writing/editing skills, retire superseded memories only after verifiable capture. Memory cleanup is the most sensitive of the four cleanup capabilities — a wrong judgment destroys knowledge rather than relocating a finished record."
binds_to_depth: phase
---

# Cleanup Memory Phase

Analyze the work store's memory records (`ctxmod work list --collection
memories`), solidify durable knowledge into the repository's context
module (`reference/` files under `context/`) via the existing
context-writing/editing skills, and retire superseded memories only
after their content is verifiably captured. The capability surfaces a
discoverable command (`/cleanup_memory`) and codifies the safety model
that makes memory cleanup safe: **verify-before-retire**.

Memory cleanup is a **phase, not a workflow**. The main agent invokes
it directly via the `/cleanup_memory` slash-command.

## Scope — the work store's memories only

The capability targets the work store's `memories` collection — its
`episode`, `fact`, `procedure`, and `navigation` records (a project's
store held 181 memory files at the capability's baseline). It does
**not** touch the harness's per-depth agent memory
(`.claude/agent-memory/<depth>/`).

This boundary is load-bearing and was reconciled against the actual
corpus before scoping (a scope-reconciliation report recorded the
decision when the capability was designed). Summary of the decision:

- **The work store's memories are the canonical project memory
  store** (the ctxmod builtin `work_discipline` profile's
  `memory_management` rule). They persist across sessions and stay out
  of the committed tree (`context.local/work/` by default).
- **`.claude/agent-memory/<depth>/` is per-depth harness scratch.** It
  is generated content that the harness resets on its own schedule;
  cleaning it via a context skill would be cleaning transient harness
  state, not durable project memory. Most of its files are
  orchestration feedback specific to one agent depth ("sub-agents may
  report success with zero git diff") that does not belong in the
  context module's reference.
- **Durable knowledge in `.claude/agent-memory/<depth>/` is already
  mirrored in the store's memories.** The few files that carry durable
  project knowledge are stale duplicates; the capability reaches them
  via the canonical store.

A future issue may address the harness-state cleanup of
`.claude/agent-memory/<depth>/` as a separate concern. That work is
not blocking and does not belong here.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- The store's memories have grown beyond what an agent can reason
  about in a single context window (181 files at baseline; growing).
- Durable knowledge buried in memory records would be more valuable as
  context-module reference content — searchable, cross-linked, and
  rendered by ctxmod for every consuming agent.
- Superseded memories (resolved bugs, completed migrations, stale
  decisions) are cluttering the listings without adding signal.

**When not to use**:

- The concern is test debt — that is the `tests` capability.
- The concern is finished work items or reports — that is the
  `archive_cleanup` capability (archival, not solidification).
- The concern is duplicated production code or compat shims — that is
  the `code-debt` capability.
- The concern is `.claude/agent-memory/<depth>/` content — out of
  scope; see the Scope section above.

## Verify-Before-Retire Gate (load-bearing)

**A memory is retired only after its content is verifiably captured in
the context module's reference.** This is the single most important
safety property of the memory capability (intent §6 Safety, AC item
25). A wrong judgment here destroys knowledge: a retired memory leaves
every default listing, so no future agent finds it, and no recycle bin
or committed git history stands behind it (the store lives outside the
committed tree). Treat retirement as irreversible even though the store
can technically restore an archived record.

The gate has three required checks, all of which must pass before a
memory is retired:

1. **Capture verification.** The durable knowledge from the memory
   must be present in a context-module `reference/` file (new or
   existing). "Present" means a reader of the reference file would not
   need the original memory to understand the knowledge. Verify by
   reading the reference file back after writing — do not trust that a
   Write succeeded (see the harness Edit-virtualization hazard in the
   agentic `shared_tree` profile's `harness_traps`).
2. **Scope verification.** The captured knowledge must be at the
   correct reference path (e.g. an orchestration package's
   `reference/<topic>.md` for orchestration guidance, a tool's package
   `reference/<topic>.md` for tool-specific rules). A capture at the
   wrong path is equivalent to no capture — the knowledge is
   unreachable.
3. **Index update.** If the reference file is new, ensure it is
   cross-linked from neighboring reference files and from any relevant
   rule or skill. An orphan reference file that no other file links to
   is functionally lost.

If any check fails, the memory is **not retired**. The capability
halts and surfaces the failure for human review.

### Retire semantics

"Retire" means **take the memory out of the live store**: set a fact
or procedure to `retracted` (`ctxmod work set <id> status=retracted`)
and archive the record (`ctxmod work archive <id>`); episodes and
navigation records, which carry no status, are archived. There is no
separate archive *step* in the work-archive sense — memory cleanup is
not the work-archive capability's job, and memories are not work
items. Retirement must be treated as irreversible; the
verify-before-retire gate is the only thing standing between durable
knowledge and loss.

### What is "durable knowledge"

A memory carries durable knowledge worth solidifying into reference
when it meets **both** of:

- **Non-derivable.** The knowledge is not reconstructable from reading
  the current code or context-module state. A memory that says "the
  bug at line X was fixed in commit Y" is derivable from git blame; a
  memory that says "this algorithm must preserve property Z because of
  constraint W" is not.
- **Reusable.** The knowledge would inform a future agent working in
  the same area. A memory that says "this one-off incident happened
  on date X" is not reusable; a memory that says "this invariant is
  load-bearing and here is why" is.

Memories that fail either test are candidates for retirement without
solidification — they have already served their purpose.

## Solidification Workflow

Memory cleanup consumes the existing context-writing and
context-editing skills rather than re-implementing reference writing.
This is the reference-over-duplication discipline (DRY/SSOT).

### Step 1 — Analyze

Read each memory in the scope (or every memory record when no filter is
given: `ctxmod work list --collection memories`, then each `file`).
Classify each memory into one of:

| Class | Criteria | Action |
|-------|----------|--------|
| **Solidify-then-retire** | Durable + reusable; not yet in the context module's reference | Capture in reference, verify, retire |
| **Already-captured** | Durable + reusable; already verifiably in the context module's reference | Retire without re-capture (verify first) |
| **Stale/superseded** | Resolved bug, completed migration, superseded decision | Retire without capture (the resolution is the capture) |
| **Ephemeral** | One-off incident, debugging note, no future value | Retire without capture |
| **Retain** | Durable but not yet solidifiable (e.g. ongoing work) | Leave in place |

The classification is the agent's judgment, not a mechanical rule.
When uncertain, default to **Retain** — a wrong retire destroys
knowledge; a wrong retain only clutters.

**Report the classification** to an analysis report record
(`ctxmod work new report "<NN>-memory-analysis"`, numbered `01`) before
proceeding. The report is the audit trail for retirements; without it,
a future contributor cannot reconstruct why a memory was retired.

### Step 2 — Solidify (per memory, sequential)

For each memory classified **Solidify-then-retire**:

1. **Identify the target reference path.** Match the memory's domain
   to the reference structure of the repository's context module
   (`context/profiles/<profile>/packages/<package>/reference/`). Common
   mappings:
   - Orchestration guidance → the orchestration package's `reference/<topic>.md`
   - Writing principles → the writing package's `reference/<topic>.md`
   - Tool-specific rules → the tool's package `reference/<topic>.md`
   - Procedures → the procedures package's `reference/<topic>.md`
2. **Consume the writing skill.** Dispatch `write_context_phase` with
   the `reference` variant to author the new file (or append to an
   existing one). Do not write reference files directly — the writing
   skill enforces density targets, file conventions, and
   cross-references.
3. **Consume the editing procedure for appends.** When solidifying
   into an existing reference file, dispatch
   `context_editing_procedure` to apply the edit with
   source-vs-override discipline (shared `context/` versus the
   device-local `context.local/` overlay).

**Solidification is sequential, not parallel.** Each capture may
affect the classification of subsequent memories (two memories may
solidify into the same reference file). Parallel solidification risks
conflicting Writes to the same file.

### Step 3 — Verify-Before-Retire (per memory, sequential)

For each memory that reached the retire step (Solidify-then-retire
after capture, Already-captured, Stale/superseded, Ephemeral):

1. **Re-read the target reference file** (for Solidify-then-retire
   and Already-captured). Verify the durable knowledge is present and
   reachable. Use `python3` direct read or `grep`, not the Edit tool,
   to confirm the file on disk matches what was written (guard
   against Edit virtualization).
2. **Check cross-links.** For a new reference file, verify at least
   one neighboring file links to it, and that `ctxmod vet` passes.
3. **Retire the memory.** Retract it where its kind has a status and
   archive the record — one `ctxmod work set` plus one `ctxmod work
   archive`, each atomic under the record's lock.

If any check fails, **halt**. Do not retire. Surface the failure to
the operator.

### Step 4 — Report

Write a completion report (`ctxmod work new report
"02-memory-completion"`) listing:

- Memories analyzed (count, by class)
- Memories solidified (count, with target reference paths)
- Memories retired (count, with reason per class)
- Memories retained (count, with reason)
- Verification results (per solidified memory: reference path, read-back
  confirmed y/n, cross-link confirmed y/n)

The report is the audit trail. Without it, retirements are
unrecoverable from the live store alone.

## Reference Target — the context module's source only

The capability writes only to the repository's shared context module
(`context/` — profile and package `reference/` files), or to the
device-local overlay (`context.local/`) when the knowledge is
machine-specific. It never writes rendered output directly (for
example a rendered `.claude/` tree): rendered files are regenerated
from the module, and direct edits vanish on the next render.

After solidification, run `ctxmod vet --root .` and render
(`ctxmod render`) to publish the new reference content to consuming
agents. This is the publication step; the authoring step happened in
the module source.

## Manual Invocation Only

This capability is manual-invocation only (intent §3 D2, §5 scope,
§7 AC item 26). **No hook, no CI, no schedule.** The operator runs
`/cleanup_memory` explicitly when the store's memory growth warrants a
sweep.

Memory cleanup is the most sensitive of the four cleanup capabilities
because a wrong judgment destroys knowledge. Archival of work items is
reversible (restore them); test deletion at worst reduces coverage;
code-debt removal is recoverable from git. **Memory retirement is
effectively irreversible** — the store lives outside the committed
tree, so there is no committed git history to recover from, and a
retired memory leaves every listing future agents read. Automated runs
without a human reviewing the analysis report risk silently destroying
durable knowledge.

The capability must not be wired into any automatic check or
background process. The verify-before-retire gate is calibrated for
human review of each retirement; an automated run would either skip
the gate (unsafe) or block on every memory (pointless).

## Error Handling

| Situation | Response |
|-----------|----------|
| A memory's durable knowledge cannot be solidified (no reference category fits) | Halt; surface to operator; default to Retain |
| Verification step fails (reference file missing or content absent) | Halt; do not retire; surface the failure |
| A memory record's state and its file disagree (`ctxmod work vet` reports a finding) | Halt; surface the drift; do not retire either until reconciled |
| A memory references another memory (by id or `[[name]]`) that does not exist | Note the dangling link; do not block retirement of the source |
| Operator cancels mid-sweep | Halt; do not continue retirement; already-retired memories stay retired |

Error handling follows stop-report-propose-await pattern. When the
capability halts, the analysis report preserves the classification for
a later retry.

## Quality Criteria

| Criterion | Validation Method |
|-----------|-------------------|
| Scope boundary honored | Capability inspects the store's memories only; never reads or writes `.claude/agent-memory/<depth>/` |
| Verify-before-retire gate enforced | Every retired memory has a verified capture in the context module's reference OR an explicit non-capture class (Stale/Ephemeral) with rationale |
| Writes only to the context module's source | No writes to rendered output, no writes to memory bodies (state changes only, through `ctxmod work`) |
| Consumes existing skills | Solidification dispatches `write_context_phase` (reference variant) and `context_editing_procedure`; does not write reference files directly |
| Reference-over-duplication | Body links to writing/context-editing skills rather than re-documenting their procedures |
| Manual invocation only | No hook, CI, or schedule wiring anywhere in the body |
| Audit trail | Analysis report and completion report written for every sweep |

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Retiring a memory without verifying the capture | Run the verify-before-retire gate; read the reference file back |
| Solidifying into rendered output directly | Write to the context module source; vet and render |
| Re-implementing reference writing | Dispatch `write_context_phase` with the `reference` variant |
| Parallel solidification into the same reference file | Solidify sequentially; each capture may inform the next |
| Touching `.claude/agent-memory/<depth>/` | Out of scope; leave harness scratch alone |
| Trusting that a Write succeeded | Verify with `python3` direct read or `grep` (Edit virtualization hazard) |
| Auto-advancing from analysis to retire without operator review | The operator reviews the analysis report; the capability does not chain |
| Running the sweep without writing the analysis report first | The report is the audit trail; without it retirements are unrecoverable |

## Sub-Layer Launch

This phase has no objective or procedure sub-layer of its own. The
capability delegates solidification to the existing
`write_context_phase` (which has its own objective and procedure
layers) and editing to `context_editing_procedure` — both in the
`authoring` profile of the writing_context module
(`github.com/ctxmod/writing_context/context`). There is no
memory-specific objective or procedure to extract — the analysis and
classification logic lives in this phase body, and the writing and
editing mechanics are already covered by the writing/context-editing
skills.

If a future caller wants the capability invoked from a workflow,
launch the `cleanup_memory_phase` agent directly:

Brief template for the `cleanup_memory_phase` agent — Execute the memory cleanup phase:

```text
<ctxmod_activate skill="agentic_work_cleanup_memory_phase" />
Analyze the work store's memories with the operator's filter, classify
each memory, solidify durable knowledge into the context module's
reference via the writing and context-editing skills, and retire
memories only after the verify-before-retire gate passes. Write the
analysis report before any retirement. Do not touch
.claude/agent-memory/<depth>/.
```

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />
`memory_management` in the ctxmod builtin `work_discipline` profile
`work_store_first` in the ctxmod builtin `work_discipline` profile
