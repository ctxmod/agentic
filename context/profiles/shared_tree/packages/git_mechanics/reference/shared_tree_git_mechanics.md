# Shared-Tree Git Mechanics

Host-level git and harness behavior that bites during commit rounds in
a repository several agents share (and in sibling repositories that
install the same command guard and pre-push hook). For the
concurrent-agent failure modes these mechanics interact with, see
`shared_git_index_hazards` in the ctxmod builtin `commit_discipline`
profile; for the standing commit/push rules, see the repository's own
commit workflow. Where a repository installs a command guard that
rewrites commits to a managed commit runner, that runner's own
documentation owns the commit shape; the mechanics below hold either
way.
For the resolution procedure when a merge or rebase stops on conflicts,
see `merge_conflict_resolution` in the ctxmod builtin `commit_discipline` profile.

## Unstaging: `git rm --cached`

Where a command guard blocks `git restore --staged` and `git reset`,
`git rm --cached <path>` passes and is non-destructive (the file stays on
disk as untracked; a tracked peer file is untouched on disk). A commit
runner whose precondition is a verified-clean index blocks the round
when a peer file slips into the shared index inside your commit window
(it names the offender rather than sweeping it): unstage the peer file
first, then commit in one command to close the race:
`git rm --cached <peer-file> && git commit -m ... -- <paths>`.

## Pathspec commits take working-tree state, not staged state

`git commit -- <pathspec>` does NOT limit itself to what you staged — it
commits the CURRENT WORKING-TREE state of every TRACKED file matching the
pathspec, bypassing the index for those paths. On a shared tree, a
dir-level pathspec (`git commit -- pkg/go/<pkg> tools/<tool>`) sweeps
any peer's unstaged modifications under those dirs into your commit; it
has landed exactly that (a peer's 2-line comment reflow rode a commit
despite never being staged). Rules:

- Enumerate every file explicitly in the `git commit --` pathspec when
  the tree is shared (a managed runner stages exactly that list). Never pass
  a bare directory unless `git status --porcelain -- <dir>` proves the
  only modifications under it are yours.
- A dir-level pathspec landing the right file count is luck, not
  discipline.
- `git ls-files <path>` exits 0 even when it matches nothing — empty
  output means UNTRACKED, so `git ls-files <p> && echo tracked` is a
  false-positive tracking check.
- For **shared-dirty files** (peer hunks in the same file as yours, so a
  pathspec commit still sweeps them): lift the peer's worktree content
  out, commit your clean version, then restore their content
  byte-identically (Read it first, re-apply, sha256-verify the restore).
  A full-file `git add` sweeps the same way when the peer edits between
  your read and your add — check `git status --porcelain` of each path
  immediately before staging.

## Wholesale-gitignored directories

Agent-state directories — a local work store, `.claude/`, `.codex/` —
are typically wholesale gitignored (with a handful of tracked
exceptions). Three consequences:

1. `git add -- .claude/...` (or another wholesale-ignored path) errors with "The
   following paths are ignored ..." even for TRACKED modified files, and
   refuses without `-f`. Do not `-f` repo-convention-ignored paths.
2. The failing add still stages the valid pathspecs before erroring —
   check `git diff --cached --name-only` after the error; the index is
   usually already correct.
3. `git commit -- .codex` errors: commit pathspecs must match at least
   one tracked file, and `.codex/` has zero. Prune commit pathspecs to
   prefixes that actually hold tracked changes.

Clean escape: the partial-commit pathspec form (`git commit -m ...
-- <explicit tracked paths>`, which commits working-tree content without
a separate staging step, or is rewritten to a managed runner that stages
internally). `git add -u -- <dir>` describes the tracked-only staging
semantics the same way, but a command guard may deny it as an agent
verb.

## Git stash

- **Never use `git stash` to baseline lint/test output mid-task.** A
  stash captures whatever concurrent state is in the tree (it once
  captured 110 files from a parallel session), and `pop` can fail,
  stranding or wiping every tracked-file edit (untracked files survive,
  tracked edits do not). To baseline: `git diff <path>`, `git show
  HEAD:<path>`, a clean-HEAD clone or archive+init snapshot baseline
  (see
  `isolation_by_checkout` in the ctxmod builtin `no_worktrees` profile),
  or just compare output counts in notes. The cost of stash (potential
  total work loss) always exceeds the benefit (a one-shot comparison).
- **A command guard that blocks stash typically matches the literal substring `git stash`**,
  including read-only `git stash list` — and it denies the WHOLE
  command, so `git log ... && git status ... && git stash list` loses
  the innocent legs too. Never chain it.
- To check whether the stash stack is empty, test for the existence of
  `.git/refs/stash` (git deletes the ref when the last entry is popped;
  absence means empty stack). Do NOT read `.git/logs/refs/stash` as a
  proxy for current state — that reflog is append-only and survives
  every pop/drop; use it only to date historical stash activity.

## Output reliability: fabricated failures and wrapper decoration

- **Failure-looking git/build output is frequently fabricated.** Advisory
  messages (a "Prefer '<gate> check ...'" hint, "Stage and commit in the
  same command", a `FAILED: git add` line with a gitignore hint) may describe
  an operation that ALREADY SUCCEEDED underneath — 43 explicitly staged
  files reported `FAILED: git add`, yet `git diff --cached` showed all
  43 staged; only the `&& commit` leg was skipped. After any suspicious
  advisory or terse `FAILED:` line, verify actual state with read-only
  commands (`command git diff --cached --name-only`,
  `command git log --oneline -1`, `git rev-parse HEAD`) BEFORE retrying.
  Never blind-re-run a `git add` after one — re-adding can sweep a
  concurrent agent's mid-operation files.
- **Piped git output through an output-rewriting wrapper can come back false-empty**:
  `command git diff --cached | grep -cE ...` printed nothing (not even 0)
  while the staged blob verifiably contained matches. Scan the staged
  blob directly (`command git show :<path>`) or per-file diffs instead of
  one giant piped `git diff --cached | grep`.
- **A git wrapper can decorate diff output**: `git diff --cached
  --name-only` includes an empty line and a `--- Changes ---` header, so
  `| wc -l` overcounts paths by 2-3. Count with
  `git diff --cached --numstat | wc -l` or `git show --stat`.
- `git diff -- <path>` can be mangled by such a wrapper — use
  `git diff HEAD -- <path>`.
- A plain `grep -c` through the Bash tool can render "0 matches"
  spuriously on patterns containing quotes/parens — Read the file region
  before concluding content is absent.

## index.lock

`fatal: Unable to create '.git/index.lock': File exists` with live peers
means a peer's git process is mid-operation — do NOT delete the lock;
re-check and retry once it clears (observed clearing within ~20-30 s).
The exception: a hook (e.g. biome-check) that hangs many minutes on a
dirty tree and dies silently can orphan a large (~166 KB) index.lock
with ZERO live git processes in `ps` — that lock is stale: `rm -f
.git/index.lock && git commit ...` in the same command, re-verifying the
index first (peers stage follow-up files while your hooks run).

## timeout cannot wrap the `command` builtin

`timeout 120 command git push` fails with "failed to run command
'command'". For a timeout-wrapped invocation use the real binary:
`timeout 120 /usr/bin/git push --no-verify`. Plain invocations keep the
`command git` compliant form.

## Parallel implementation, sequential commits, one shared tree

When a workflow wants concurrent implementation agents, the default is
ONE shared tree with strictly sequential commits: parallel workers
write disjoint files, and each commits by explicit pathspec when its
unit completes (the pathspec-commit rules above). Where worktree
isolation is banned (shell commands and harness `isolation` params
alike), a separate clean-HEAD checkout — clone or archive+init
snapshot — is the sanctioned isolation, and it does not share the main
repository's ctxmod work store (a separate clone anchors its own
`context.local/work/`), so records created there do not propagate.
A checkout that is not at the intended base voids the work built on
it
(`isolation_by_checkout` in the ctxmod builtin `no_worktrees` profile).
Mid-run strategy reversals kill
in-flight agents and strand WIP — pick the model at planning time. In a
shared-tree relay, never satisfy a compile dependency by materializing
a peer's file: the successor's commit absorbs an interface it does not
own (stub interference). Sequence the work instead, or stub-and-document
locally without committing the stub.

## Commit-message files and round waits

A commit-msg guard can reject a one-line `git commit -m "subject"`
("commit message arrived as one fused line") — a message must carry
subject, blank line, body, and trailers. Build the message file up
front (Write tool or printf) and pass `-F /tmp/msg.txt`; the
repository's own commit workflow owns its byte-exact trailer contract.
Two round ergonomics facts from the same waves:

- A PreToolUse sleep guard can block consecutive `sleep` calls AND
  `for`-loop sleep polling in Bash. To wait on a detached process
  (e.g. a detached `git push` behind the pre-push gate), block on the
  PID: `tail --pid=<pid> -f /dev/null` — passes the guard and returns
  when the process exits.
- `git diff` output through a decorating shim is reformatted (section
  headers, indented hunks, `--- Changes ---` footers) and unusable as
  `git apply` input — generate patches with the unwrapped git
  (`/usr/bin/git diff`, or the wrapper's raw pass-through mode).

<!-- anchor: commit-round-msg-file-and-waits -->

## `git commit --only` and the partial-commit index

During a `git commit --only` pre-commit hook, `GIT_INDEX_FILE` points
at git's private partial-commit index, so a hook's plain `git add
<file>` lands in the partial index and ENTERS the commit
(lint-staged's whole model). To model a peer staging into the SHARED
index mid-round, the hook must target the real index explicitly:
`GIT_INDEX_FILE="$(git rev-parse --git-dir)/index" git add ...` — and
even then git's final index rewrite may clobber a mid-hook real-index
add: the peer entry is excluded from the commit, but whether it stays
staged is git's outcome, not the runner's. Assert "appears in no
commit", never "still staged", when testing sweep immunity or writing
hooks around `--only` commits.

Two adjacent mechanics: `git diff --cached --name-only`, `git
ls-files`, and pathspecs are all cwd-relative, so index probes keyed
for cross-round comparison (journals, ownership checks) must run with
the repo root as cwd — EvalSymlinks both root and cwd first
(macOS /tmp-style symlinked cwds). And a `-F <file>` placed after `--`
parses as a pathspec, aborting the commit with "empty commit message"
(exit 1) — order args as `commit -F <file> [--amend] --only -- <paths>`.

<!-- anchor: only-commit-partial-index -->

## Pre-push gate wedges look like hung pushes

A pre-push hook that runs a diff-scoped check gate BEFORE the
transport can make a push hang 10-22 minutes with zero output and look
like a network stall when the gate's index read is wedging under peer
load. Treat a stuck push as a stuck GATE: when the gate's error is
retryable and its failing scope advances across retries, retry
patiently. Kill your OWN stacked stuck push processes first — exact
PIDs only, a `pkill -f` pattern self-matches your own compound command
— and verify no leftover `git push` processes before starting a new
one: `timeout <n> git push` does not bound the gate (wrapper and gate
children can survive SIGTERM), so bounded-retry loops stack stuck
attempts, and the landing often comes on the first attempt after the
stack is cleared. Gate-survival ladders under peer load live with the
gate's own documentation.

<!-- anchor: prepush-gate-before-transport -->

## Stale origin tracking and the ref-push race

The remote-tracking ref is only as fresh as the last fetch: fetch
origin and re-read `origin/main` before acting on any "N unpushed
commits" count in a multi-agent tree. A task brief claiming work is
uncommitted and unpushed can be stale in both dimensions — check
scoped `git status` for the claimed dirty set and `git log -- <scope>`
for a peer commit already carrying the work. A push rejected with
`cannot lock ref 'refs/heads/main': is at <our local HEAD> but
expected <stale tracking hash>` is the race's signature: when the
"is at" hash equals your own HEAD the push is a no-op, not a conflict
— fetch, confirm `0 0` ahead/behind, and report already-synced
instead of retrying. This is the push-side twin of the staging races
in `shared_git_index_hazards` in the ctxmod builtin `commit_discipline` profile.

<!-- anchor: stale-origin-ref-push-race -->

## Mid-validation plan supersession

Peer commits can land an entire validated commit plan while its
validation pass is still running (all 84 paths of a 7-commit plan
landed via six peer commits during one round-5 validation): HEAD
moves and every plan path goes clean before the report is written.
Re-run `git status --porcelain=v1` and the HEAD check as the LAST
evidence step, immediately before writing the report — the
validation's supersession verdicts expire just as fast as the plan's.
If plan paths went clean, map each plan commit to its landing commit
via `git log <base>..HEAD` plus `git show --stat` (per-file diff-stat
identity matching the measured working-tree deltas proves content
identity) and report the plan as superseded with a no-op directive
rather than validating a ghost. Sweep-group suffixes
(`(sweep group-NNN)`) are real and consumed serially — check recent
history before proposing a group number. `wc -l` on a status snapshot
undercounts by one when the last line lacks a trailing newline.

<!-- anchor: midvalidation-plan-supersession -->

## Guard-denied worktree removal leaves prunable registrations

A command guard can deny `git worktree remove`, `prune`, `move`,
`repair`, and `add` (they mutate shared `.git/worktrees` metadata in a
repository used by concurrent agents), but plain
`rm -rf` of a worktree DIRECTORY is not guard-covered: afterwards
`git worktree list` marks the registrations `prunable`, git stays
consistent, and a single operator-run `git worktree prune` clears
them. For operator-directed cleanup: run the safety pass first (proc
cwd scan, lock check, <12h mtime check), `rm -rf` the directories,
then hand `git worktree prune` to the validator/operator as a one-line
residue instead of improvising around the guard. The worktree ban and
baseline doctrine:
`isolation_by_checkout` in the ctxmod builtin `no_worktrees` profile;
the work-reference ID-token veto and the multi-`-m` trailer-split traps
hit during the same round belong to the repository's own commit
workflow.

<!-- anchor: worktree-remove-prunable-residue -->
