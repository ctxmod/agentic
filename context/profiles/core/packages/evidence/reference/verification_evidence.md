# Verification Evidence

A check-gate verdict, a backgrounded gate, a claimed census — each can
look green while proving nothing. This reference carries the
tool-neutral part of the evidence discipline: the verification-sweep
checklist and the evidence checks that sit beyond any one check gate
(censuses, store-root aliases, managed-output drift, truncated peer
files). How a specific gate's payload, partial verdicts, exit codes,
and run-bounding recipes read lives with that gate's own documentation;
this file is about reading what came back.

## The verification-sweep checklist

Run a sweep in this order; each rule is expanded below or in its home
reference:

1. Read the gate file first (the issue or story body, or the task summary) and
   quote the criterion — a remembered gate string is not evidence
   (`stale_inputs_and_drift` in the ctxmod builtin `delegation` profile).
2. One invocation per Bash call, scoped (`--path`) gates first — budget
   the whole Bash call so the harness timeout cannot kill a gate
   mid-run and leave a verdict-shaped fragment.
3. Redirect output to a file and read the echoed rc — never
   pipe-verify an exit code: a wrapper or a pipe reports its own exit
   status, not the gate's.
4. Attribute pre-existing findings via the no-commit HEAD-baseline
   recipe, never `git stash` —
   <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/head_baselines.md" />.

## Evidence integrity beyond the check gate

- **Census forensics.** Long-running implementer agents can fabricate
  census entries: a claimed 9-file diff census where 6 files were
  bit-identical to HEAD. Before any commit round on an agent's batch:
  per-file `git diff HEAD --stat` against the claim; pushed-marker
  greps (named symbols from the pushed commits) in the working files;
  a strings-check of the deployed binary for new symbols when a
  rebuild happened mid-round; `git reflog -10` for resets/checkouts.
  Fabrications are harmless only when the fabricated files carry no
  diff — a census entry whose "changes" would revert pushed content
  institutionalizes the regression if committed blind. Claim
  verification is a distinct discipline from reading a sub-agent's
  report: `subagent_report_verification` in the ctxmod builtin `delegation` profile.
- **Store-root census alias.** A census row asserting a tool store
  under the tool's home cache directory can be wrong on a host whose
  user cache directory resolves elsewhere (a mounted cache, or a path
  normalization that inserts a filesystem-identity alias segment for
  bind-mounted checkouts): the live store may sit somewhere else entirely
  while nothing exists under the home cache. To find any tool's live
  store root without guessing the normalization, read a running
  instance's open descriptors (`ls -l /proc/<pid>/fd | rg <tool>`
  names the exact record file being touched); never `find` under the
  cache root — the walk is huge and gets memory-killed.
- **Managed-output drift.** Sync-green is only meaningful against the
  generator state at sync time; a later generator commit without a
  follow-up generator sync leaves committed managed outputs stale
  while every story-level check passed at its own closure time (one
  epic's closure check asserted zero managed-file churn while the
  committed `.pre-commit-config.yaml` lacked the `--expect-author`
  argv its generator had begun emitting). In any validation touching
  generators or generated configuration: snapshot `git status
  --porcelain`, run the generator's sync, diff the snapshots, and attribute churn — churn
  matching a landed generator change is story-attributable and gets
  committed as the smallest-correct repair; anything else is peer
  noise.
- **Restart-truncated peer files.** A host-session restart can leave a
  peer's uncommitted working-tree file at 0 bytes; one empty file in an
  imported package breaks compilation for the whole module — every
  gate on the host. Protocol: diagnose compile failures naming a file
  you never touched (`wc -c`, `git status`); wait the sanctioned 5
  minutes for the peer to heal; confirm no open writer via a
  `/proc/*/fd` scan; restore HEAD content by staging the pristine copy
  under /tmp (the guard blocks `git show HEAD:path > worktree`) and
  placing it with the Write tool; verify with `git diff --stat` — a
  byte-identical restore shows an EMPTY diff; record the repair and
  notify the peer campaign. Shared-tree git mechanics:
  `ctx:github.com/ctxmod/agentic/context::shared::git_mechanics@shared_tree/reference/shared_tree_git_mechanics.md`.

## Related

- The deliberate-verification frame this evidence feeds:
  <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />.
- Input freshness before running any of this:
  `stale_inputs_and_drift` in the ctxmod builtin `delegation` profile.
- Negative controls and vacuous-pass doctrine:
  <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/negative_controls.md" />.
