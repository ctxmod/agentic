---
description: "Acting phase for the code-debt cleanup capability. Fans out the analysis objective per target, enforces the verify-before-remove gate (no removal until every reference is enumerated and updated), applies removals and reference updates sequentially, runs behavior-preservation checks. Code-debt mutates shipping code — deeper verification than tests cleanup."
binds_to_depth: phase
---

# Cleanup Code-Debt Phase

Acting phase for the `code-debt` cleanup capability. Targets three
categories of accumulated debt in shipping code — compat shims,
reference-preserving duplicate files, and DRY/SSOT violations — and
removes them **without breaking behavior** via a verify-before-remove
gate.

Code-debt cleanup is a **phase, not a workflow**. The main agent
invokes it directly via the `/cleanup_code_debt` slash-command.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- A compat shim, deprecated alias, or migration layer has outlived
  its migration window and every consumer can switch to the
  canonical location.
- A duplicate file exists solely so references do not break, and the
  references can be updated to point at the canonical source.
- The same function, constant, or map is defined in multiple places
  and should converge on a single source of truth.

**When not to use:**

- The concern is test debt (redundant, flaky, or implementation-pinning
  tests) — that is the `tests` capability. Deleting a test cannot
  break production; deleting a shim can.
- The concern is work-store archival — that is `archive_cleanup` (agentic `work` profile).
- The concern is solidifying memories into reference — that is
  `memory`.
- The target is a one-off bug fix or refactor with no duplication or
  compat-shape concern — use `/implement_refactor` directly.

## Why Code-Debt Is the Most Dangerous Cleanup

Of the four cleanup capabilities, code-debt is the one that mutates
**shipping code** (intent §3 D2 rationale, §6 Safety):

- **Tests cleanup** deletes test files. Worst case: reduced coverage.
  Production behavior is untouched.
- **Work-archive** archives work-store records (`ctxmod work archive`).
  Worst case: an item is archived prematurely; the move is reversible
  (`ctxmod work restore`).
- **Memory cleanup** deletes memory files. Worst case: durable
  knowledge is lost. The work store lives outside the committed tree
  (`context.local/work/`), so recovery depends on its checkpoints and is
  hard, but
  production code is untouched.
- **Code-debt cleanup** removes production code. Worst case: a stale
  reference is missed, a build breaks, a call site silently falls
  through to wrong behavior. Recovery requires a rollback.

This is why the verify-before-remove gate is load-bearing and why
this capability has a depth split (objective for analysis, phase for
action) that the simpler cleanup capabilities do not need.

## Three Categories

The three categories are distinct. The phase does not collapse them.

### 1. Compat Shims

Backward-compatibility or migration layers that outlived their
migration window. Type aliases pointing to new locations, wrapper
functions that call through to newer implementations, deprecated
re-exports kept so old references compile.

**Removal preference:** update every reference to point at the
canonical location, then remove the shim. Keeping the shim to avoid
updating references is the exact shortcut this capability exists to
remove.

### 2. Reference-Preserving Duplicate Files

Files duplicated across two paths, where the duplicate is kept solely
so references (imports, build paths, documentation links) do not
break.

**Removal preference:** update every reference to the canonical path,
then delete the duplicate file. If the duplicate has drifted from the
canonical source (local edits not reflected upstream), halt for human
reconciliation — removing it would lose those edits.

### 3. DRY / SSOT Violations

Duplicated functions, constants, or maps that should converge on a
single source of truth. Unlike the duplicate-files category, the
duplication is at the symbol level within otherwise-distinct files.

**Removal preference:** choose the canonical definition site (lowest
layer in the package dependency graph wins), update every call site
to use the canonical definition, remove the non-canonical definitions.
A call site may keep its local copy only with explicit justification
(e.g. avoiding an import cycle).

## Verify-Before-Remove Gate (load-bearing)

**No removal happens until every reference is enumerated and updated.**
This is the single most important safety property of the code-debt
capability (intent §6 Safety, AC item 23). A removal that proceeds
with an incomplete reference set is the failure mode the gate exists
to prevent.

The gate has four required checks, all of which must pass before a
removal proceeds:

1. **Reference enumeration completeness.** The analysis objective
   enumerated references via **both** the symbol index (definition-level
   code intelligence) and lexical search (ripgrep, fd, ast-grep).
   Neither alone is sufficient — the symbol index misses lexical
   patterns (comments, strings, non-indexed file types); lexical search
   misses semantic relationships
   (call vs. definition vs. string literal). The merged, deduplicated
   reference set is the authoritative input.
2. **Reference update applied before removal.** Every reference in
   the enumerated set is updated to point at the canonical target
   **before** the shim, duplicate, or non-canonical definition is
   removed. Out-of-order execution leaves dangling references that
   break the build.
3. **Behavior preservation verified.** The removal does not change
   observable behavior. A wrapper that adds caching, validation, or
   logging is not a pure shim — its removal changes behavior and must
   be flagged for human review, not auto-removed. For DRY/SSOT
   violations, verify the canonical definition's signature and
   semantics match every call site's expectation.
4. **Build + test pass after removal.** The capability runs the
   repository's scoped check gate (or the relevant build command) and the package
   test suite after every removal. A build failure or test regression
   blocks the removal from being considered complete — the operator
   must review the failure before the capability proceeds.

If any check fails, **halt**. Do not proceed to the next removal.
Surface the failure in the apply report and await operator review.

### Preference: update references over keeping shims

The explicit goal is removing shortcuts **without breaking behavior**
(intent §6). When the choice is "update the references" vs. "keep
the shim so references don't break," the capability prefers updating
the references. Keeping a shim to avoid updating references
manufactures permanent production complexity — the exact debt this
capability exists to remove.

The exception is a reference that cannot be updated mechanically
(cross-layer import that would violate the layer model, signature
mismatch that requires redesign). Such references are flagged for
human review; the shim is not removed until they are resolved.

## Phase Orchestration

Three phases, sequential. The discovery phase identifies targets; the
analysis phase fans out one objective per target; the apply phase
enforces the verify-before-remove gate and executes removals.

```
Phase 1: Target Discovery
         ↓  (one or more code-debt targets)
Phase 2: Per-Target Analysis (one dispatch per target)
         ↓  (one cleanup_code_debt_analyze_objective per target)
Phase 3: Apply (sequential, verify-before-remove gate)
         ↓
      Complete
```

### Phase 1: Target Discovery

If the operator passed a `target` argument (file path, symbol, or
scope), skip discovery and dispatch analysis directly for that
target. Otherwise, identify candidates via:

- the repository file index's per-file record filtering for files tagged `generated` or
  carrying deprecated markers
- lexical search for `// Deprecated:` comments, `type X = ` aliases,
  and near-duplicate file pairs
- the symbol index for symbols defined in multiple locations

Report candidate targets to
`{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery.md` with one entry per
target and a preliminary category guess. Do not enumerate references
here — that is the analysis objective's job.

### Phase 2: Per-Target Analysis (one dispatch per target)

Fan out **one `cleanup_code_debt_analyze_objective` per target**. Each
objective receives its target descriptor as INPUT_REPORT.

- Variant: analyze
- Input: target descriptor (file path, symbol, or scope)
- Report per target: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-analysis-{target-id}.md`

**Dispatch discipline:** this phase agent is **not** granted
`allow-parallel-agents: true` — it removes code in the apply phase, and
concurrent removal is the failure mode the sequential default guards
against. Launch one target's objective at a time, in the foreground,
waiting for each. Targets are independent — their reference sets do not
overlap until the apply phase consolidates findings — so sequential
dispatch costs turns, not correctness. If two targets share references
(e.g. two shims in the same package), the apply phase detects the
overlap and sequences their updates. If the concurrency instruction in
your own agent-depth instructions ever grants concurrent dispatch, the
Phase 2 analysis fan-out (read-only) may use it; the apply phase stays
sequential regardless.

### Phase 3: Apply (sequential, verify-before-remove gate)

Read every per-target analysis report. Consolidate findings and
execute per target, **sequentially**:

1. **Verify-before-remove gate** (per target, before any removal):
   - Confirm the reference enumeration is complete (symbol index +
     lexical search both ran, results merged).
   - Confirm every reference has an update target specified.
   - Confirm behavior-preservation-risk is `none` or `low`. Medium
     or high risk routes to human review — do not auto-remove.
2. **Apply reference updates** (per target, before removal):
   - Update every reference site to point at the canonical target.
   - Updates are mechanical: import swaps, type renames, path
     rewrites. Non-mechanical updates (signature changes, cross-layer
     imports) are flagged for human review.
3. **Apply removal** (per target, after updates):
   - Remove the shim, duplicate file, or non-canonical definition.
4. **Build + test check** (per target, after removal):
   - Run the scoped check gate over `<prefix>` (or the relevant build command) and the
     package test suite.
   - A failure blocks the removal from being considered complete.
     Surface the failure, await operator review.

**Corpus-safety gate**: if the consolidated removal set would touch
more than 10 distinct files (a heuristic threshold; tune per repo),
halt and request human review of the consolidated plan before
applying any removal. A large removal set signals either an
overly-broad discovery pass or a corpus that has drifted — both
warrant human eyes.

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-apply.md`

The apply report carries:

- Targets analyzed (count, by category).
- Targets removed (with the canonical target each consolidated to).
- References updated (with file:line per update).
- Findings flagged for human review (with the reason for flagging).
- Build + test results per removal.
- Verify-before-remove gate status per target (pass / halt-with-reason).

## Information Flow

```
Phase 1: Target Discovery
- Receives: {inherited work item context}, target argument (optional)
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-discovery.md

Phase 2: Per-Target Analysis (one dispatch per target)
- Receives: {inherited}, Phase 1 output (or target argument directly)
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-analysis-{target-id}.md
  (one per target)

Phase 3: Apply
- Receives: {inherited}, every Phase 2 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-apply.md
```

Main agent passes report **paths** between phases, never report
content. Each phase reads prior phase report paths as INPUT_REPORTs.

## Manual Invocation Only

This capability is manual-invocation only (intent §3 D2, §5 scope,
§7 AC item 26). **No hook, no CI, no schedule.** The operator runs
`/cleanup_code_debt` explicitly when code debt is suspected. The
capability must not be wired into any automatic check or background
process — removing production code demands a human decision per
sweep, and the verify-before-remove gate is calibrated for human
review of each removal.

## Error Handling

| Situation | Response |
|-----------|----------|
| Discovery finds zero targets | Report and exit (nothing to do) |
| Analysis objective reports low confidence on a target | Route to human review; do not auto-remove |
| Analysis reports medium or high behavior-preservation-risk | Route to human review; do not auto-remove |
| A reference cannot be updated mechanically (cross-layer, signature mismatch) | Flag for human review; do not remove the shim/duplicate until resolved |
| Build fails after a removal | Surface the failure, leave the removal in place for inspection, halt subsequent removals |
| Test regression after a removal | Surface the regression, halt; the removal may have changed behavior despite classification |
| Corpus-safety gate trip (>10 files touched) | Halt, request human review of consolidated plan |
| Two targets share references (overlapping update sites) | Sequence their updates; apply shared updates once |

Error handling follows stop-report-propose-await pattern.

## Quality Criteria

| Criterion | Validation Method | Phase |
|-----------|-------------------|-------|
| Three categories treated distinctly | Analysis report classifies each target; phase applies category-specific verification | 2, 3 |
| Verify-before-remove gate enforced | Every removal has a complete reference enumeration (symbol index + lexical search), applied updates, behavior check, build+test pass | 3 |
| Symbol index and lexical search both consumed | Analysis report cites both tools; reference set is merged | 2 |
| Update references preferred over keeping shims | Body declares the preference; apply phase updates references before removal | 3 |
| Build + test pass after every removal | Apply phase runs build and tests per removal; failure blocks completion | 3 |
| Corpus-safety gate | Apply phase computes removal-set size; halts above threshold | 3 |
| Manual invocation only | No hook, CI, or schedule wiring | all |

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Removing before enumerating every reference | Verify-before-remove gate: enumerate first (symbol index + lexical search), update, then remove |
| Running only the symbol index or only lexical search | Run both; merge results; neither alone is sufficient |
| Keeping a shim to avoid updating references | Update the references; that is the capability's purpose |
| Auto-removing medium/high behavior-preservation-risk findings | Route to human review |
| Removing concurrently when targets share references | Sequence updates; apply shared updates once |
| Skipping the build + test check after removal | Run build + tests per removal; a failure blocks completion |
| Trusting that an Edit succeeded | Verify with `git diff` or direct read after the update batch |
| Collapsing the three categories into one generic "debt" bucket | Verify steps differ per category; classify first |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
paired analysis objective.

Phase 2 (one dispatch per target, sequential by default):

Brief template for the `cleanup_code_debt_analyze_objective` agent — Analyze code-debt target <target-id>:

```text
<ctxmod_activate skill="cleanup_code_debt_analyze_objective" />
Variant: analyze
Input: {discovery-report-path or target descriptor}, target-id <target-id>
Enumerate every reference via the symbol index (definition-level code
intelligence) and lexical search (ripgrep, fd, ast-grep). Classify the category.
Propose removal + reference-update plan.
Report to {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-analysis-{target-id}.md.
```

Phase 1 (discovery) and Phase 3 (apply) are executed by the phase
itself — discovery is a targeted query, and apply enforces the
verify-before-remove gate and executes removals directly. Neither
dispatches an objective.

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly as
a "clean up code debt" sub-step. Launch the `cleanup_code_debt_phase`
agent from any workflow that needs code-debt removal capability.

Brief template for the `cleanup_code_debt_phase` agent — Execute the code-debt cleanup phase:

```text
<ctxmod_activate skill="cleanup_code_debt_phase" />
Execute the full discover → analyze → apply chain with the
verify-before-remove gate enforced at every removal. Target:
<target or repo-root>. Do not remove any code until every
reference is enumerated and updated.
```

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped guidance for the sub-agents this phase launches:

the `cleanup_code_debt_analyze_objective` skill
