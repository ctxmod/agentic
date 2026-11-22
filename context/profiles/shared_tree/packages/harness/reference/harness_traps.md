# Harness Traps

Harness and tooling layers around the agent — Edit virtualization, command guards, shell shims, wrapper scripts, harness lifecycle kills (watchdog, memory guard, SIGTERM exit semantics), evidence-corrupting verification channels — can silently invalidate work or fabricate success. The traps below recur; each check is cheap and load-bearing.

## Edit-tool virtualization

The Edit tool can report success and a following Read can show the edited content while the on-disk file is unchanged — builds and tests then run against the unmodified file and pass misleadingly. Detection signals:

- `git diff --stat <path>` returns empty when a diff was expected.
- `git status` does not list the file as modified.
- The harness grep returns zero matches for tokens just inserted (the grep tool can be virtualized too — read the disk directly: `python3 -c "print(open('<path>').read().count('<token>'))"`).
- Test count is identical to baseline after tests were added.

Recovery: rewrite the change via a Python script or `cat > file <<'EOF'` heredoc through Bash, which bypasses the Edit layer and persists. Run validation twice — once after editing (may be virtualized) and again after confirming the diff is non-empty; the second run is the authoritative one.

## Edit tool strips trailing whitespace

`old_string`/`new_string` are stripped of trailing whitespace before comparison. A checkbox tick `- [ ] ` → `- [x] ` produces `- [x]Both targets…` (malformed Markdown), and the obvious repair (`old="- [x]"`, `new="- [x] "`) is rejected as "exactly the same". Anchor the edit on the following text character instead: `old="- [x]Both targets"` → `new="- [x] Both targets"`. List-item first letters are usually distinct, giving unique per-line anchors with the space placed mid-string where it survives. Always follow with a fresh Read plus a byte-level check (`tail`, `wc -l`).

The failure scales silently with `replace_all`: one whose `new_string` ends with an intended trailing space has written space-less lines across four files before anyone noticed. When no following text exists to anchor on, use the two-step sentinel: first replace the marker with `marker SENTINEL` (the space mid-string transmits reliably), then replace ` SENTINEL` with an empty string. Verify clerical ticks with pinned `/usr/bin/rg --no-ignore -n '^- \[' <dir>` plus a no-space negative pattern like `^- \[x\][^ ]` — a missing space after a tick is invisible to status-only greps.

## Edit result echoes are rendering noise

Treat an Edit tool's result echo as untrustworthy rendering; trust only the success/failure signal, `git diff --stat` magnitude versus the intended footprint, a scoped compile, and Read for coarse structure. In one sweep the echoes showed systematically corrupted identifiers while the disk was correct — and at the same time one Edit DID slip a one-line function rename (a test helper renamed into a duplicate of a second function) inside a 6+/26- stat window that matched the intended footprint exactly; rendering noise also faked "duplicate function" artifacts in Reads, so eyeballing cannot arbitrate between real and phantom corruption. The scoped compile stage caught the true redeclaration at the exact line — a compile is the only trustworthy corruption detector.

Run the scoped compile gate (for Go, `go build`/`go vet` over the touched packages) immediately after any batch of Edits instead of reasoning about echoes. For long `old_string`s prefer a python name-anchored deletion (match `^func (Test\w+)\(` through the next `^}`), which operates on disk bytes, reports MISSING loudly, and survives rendering noise. When a file is corrupted beyond incremental repair, rebuild it from `git show HEAD:<path>` with all changes re-applied in one python pass carrying positional asserts (asserts caught two depth/scope mistakes before write). Use Read for line-start function names only; never diff two Reads character-level. Distinct mechanism from Edit-tool virtualization above: there the echo and Read both lie because the disk never changed; here the disk is usually fine and the echo lies.

## The Read tool can display a phantom tail line

The Read tool can display one phantom line past the file's true end — Read reported 93 lines where awk, `wc`, and `od` agreed on 92 newline-terminated records, with mid-file headings aligned so the off-by-one stayed invisible until an end-range cut. A byte-exact range like `sed -n '78,93p'` then silently extracts one nonexistent empty line or misaligns coverage evidence, and the only symptom is a one-line count mismatch that is easy to wave off.

Before any byte-exact extraction, count the file with a second tool (`awk '{a[NR]=$0} END{print NR}'` or pinned `/usr/bin/wc -l`), map section boundaries with awk, and preview every `sed` range before applying — the verify-with-a-second-tool discipline (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::verification/rules/second_instrument_evidence.md" />). Related sandbox fact: files written to /tmp in one Bash call may not survive into a later call, so re-derive `git show` baselines in the same Bash call that consumes them.

The write side of the same corruption family bites harder: Write-tool content can land letter-drifted with rows silently missing, and read-back cannot arbitrate a write — verify generated deliverables with the filesystem-enumerated fuzzy protocol in <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::harness/reference/edit_verification_protocol.md" />.

## Destructive git is operator-run, never agent-run

A PreToolUse command guard (for example the ctxmod builtin `git_safety` profile's hooks) hard-denies destructive git — `git reset --hard`, `git rebase`, `git stash`, `git worktree add` — by design, unchanged (the shared-tree mechanics around the block list: <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::git_mechanics/reference/shared_tree_git_mechanics.md" />); the gap is briefing an agent into a guaranteed denial, not the guard. An agent dispatched to "reset/rebase to X" burns a stop, a report, and a resume round on a block that was certain from the first clause; the operator runs destructive git manually. Correct dispatch shape: keep destructive git in the operator's hands — brief the agent to stop cleanly in one round and hand back the exact command (`git reset --hard <sha>`, `git rebase ...`) drafted in its report for the operator to run.

When a denied clause rides a compound, the deny reason stays the unchanged rule message, then a clause-level diagnosis:

> "Denied clause 2 of 2 in this compound: 'git reset --hard HEAD~1'. The whole compound is blocked, so no clause ran. The clauses before it passed the guard and can be re-issued alone: 'cp X /tmp/…'."

Prefix execution was deliberately rejected (the guard's statement model is lossy — the reassembled prefix is not byte-faithful shell), so re-issue the passing prefix clauses yourself; do not expect the guard to run them.

## Scripted-sweep trap stack

Mechanical-sweep traps, each of which burned a verification cycle mid-execution:

1. **A lint-guard PreToolUse hook can match the linter's name anywhere in the Bash line.** A read-only provenance count (`find ~/.cache/golangci-lint -type f -newermt X | wc -l`, no lint invocation) was flagged; variable indirection is rejected too as bypass detection. Move the scan into a python3 script file so the Bash line carries only `python3 /tmp/.../cachecount.py`.
2. **A mechanical `sed` over a test tree can rewrite the helper it introduces.** `sed 's/t\.Fatalf(/failf(t, /g'` rewrote the failf helper's own tail call into `failf(t, ...)` — infinite self-recursion, a 400 MB append log, run killed -1. After any mechanical sed over test files, grep the rewritten helper's own body for self-matches before running anything.
3. **`rg -rn` is `--replace`, not recursive-plus-numeric** — it rewrites displayed output only (files untouched), silently corrupting grep evidence shapes (`func FilterWalkedByScope` printing as `func n(`).

## pgrep self-match deadlocks quiet gates

`while pgrep -f "gate check"; do sleep 20; done` inside a background script never exits: the script text is the shell's own cmdline, so pgrep matches the caller. Use the bracket pattern `pgrep -f "[g]ate check"` — it matches real processes but not the caller — and bound the loop with a poll cap so a residual hang surfaces as "WAIT CAP HIT" instead of spinning forever. A listing filter like `pgrep -fa "gate check" | grep -v snapshot` masks the self-match (the calling shell's cmdline contains the shell-snapshot path), which makes the bug intermittent-looking.

## pkill self-match kills the caller (exit 144)

Kill by recorded PID first: `kill -TERM <pid>`, a bounded wait, then `kill -KILL` on the same PID (see <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::harness/reference/gate_runner_pattern.md" />). A `pkill -f "<pattern>"` also matches the caller's own eval/command line, because the shell running the kill carries the pattern text in its arguments; the kill lands on the agent's own shell and the step ends at exit 144 with partial output. Exit 144 means a SIGTERM from outside the harness reached the agent's own Bash wrapper shell — harness-initiated kills report 137 or 143 instead — and the A3-INV investigation attributed every baseline exit 144 to a `pkill -f` self-match. Use a bracket pattern such as `pkill -f "[m]ock-server"` only when no PID was recorded (a server started by an earlier agent or session) and you are sure no peer runs a matching process: the bracket spares the caller but still kills a concurrent peer's shell or server whose command text matches the pattern. Treat an exit 144 during background-server work as this trap first.

The 144 mapping is harness-version behavior (Claude Code 2.1.276-2.1.281): a Bash wrapper shell that dies by signal reports 144 for SIGTERM and 1 for any other signal — 144 is not 128+16 arithmetic. The harness's own kills (timeout, output cap, teardown) resolve 137 or 143 before signaling and never show as 144; a child killed by TERM while the shell survives shows 143; background tasks report the same 144 on SIGTERM; OOM is SIGKILL and reports 1. A re-examination of thirteen exit-144 kills previously labelled "8 unexplained" attributed all thirteen to `pkill -f` self-matches; the only pkill-free reproductions found were a peer shell's pattern kill matching your command text and `kill 0`. When a command ends at 144, hunt a pattern kill — yours or a peer's — before suspecting harness teardown or OOM.

## The session watchdog kills sessions parked on one unbounded call

The session watchdog kills a session that shows no progress for 7200s (`stalled: no progress for 7200s (stream watchdog did not recover)`), and one unbounded tool call on a loaded box is indistinguishable from a hang: observed 2026-08-31 as four agents in one afternoon dying with the identical signature, each parked on a single call (a full-suite gate run including tests, a `du` over a multi-GB /tmp) at load ~11-13 on 8 cores with swap pressure. Total session runtime is irrelevant — an agent that kept making progress ran 2.1 hours fine; every stall parked on exactly one call.

Dispatch discipline under load: wrap every potentially-slow command in `timeout 600`; never run a full multi-package suite in one call (one package per call, pre-compiled test binaries from the package dir, or `--run <Name>` for a single test); require incremental checkpointing — append findings to the report file after each unit so a watchdog death leaves a recoverable trail; stagger heavy agents, since launching several validation-heavy agents into a loaded box recreates the wedging condition that kills them all. A recovery brief starts with a Step-0 inventory classifying the prior agent's partial tree state (COMPLETE / PARTIAL-COHERENT / PARTIAL-BROKEN / UNTOUCHED) before editing, and reverts only surgical hunks — never whole files mixing peer work. Under the same load the work store's lock can time out: on a failed `ctxmod work new`, retry it and surface the error if the retry fails — hand-assembling a canonical-looking report path is itself the trap: a write hook may accept conforming hand-assembled names, but the store is the SSOT for id and path minting and a path no record owns is absent to any rule-following writer.

## The harness memory guard kills background gates under memory pressure

A third background-kill mechanism, distinct from the ~60-minute harness kill and from a check gate's own wedges: when host memory runs low the harness stops harness-tracked background tasks with "stopped because the system is running low on memory" — it fires regardless of task age, and peer agent load is the trigger (observed at load ~20 with 251 MiB free and swap 90% used; again at swap 87-99% with 11 peer gates). Direct background spawns died in 1-2.5 minutes. One 600s-foreground run was killed ~23s after its auto-background transition — after `[1/1] checked .` printed but before the buffered stdout JSON flushed — losing the verdict payload, exit file never written; the same command in the foreground-cap shape (`timeout 600000`, auto-background continuation) completed on its next execution (exit 0, 20m17s): foreground buys at least 600s of genuine execution before the exposure window.

Read the kill *reason* in the task notification, not just "killed" — a memory-pressure kill is environmental, not a gate verdict, and is never a test failure. Check `free -m` and `uptime` before launching a gate (swap >80% and load ~20 means expect the kill); the response ladder is retry ONCE, then negative-controlled staticcheck plus an honest gap — a second OOM kill means stop attempting gates. Kernel-level work still completes: a killed run may have finished its stages and lost only the buffered report — redirect stdout/stderr/exit to files, and if stdout was re-bound by the auto-background transition the report may be intact in the original redirect file. Treat a "0-byte JSON" as unverified and `ls -la` the file: a `wc -c` shim reported a false 0 on a 91.6K report. Where a gate streams stage events on stderr, stage outcomes are recoverable from that stream even when the JSON is lost — but stages-only is never a test verdict.

Two co-facts from the same incidents. First, the gate under the same pressure spent ~406s in cache setup acquiring its run-sequence lock before being interrupted, so even the compile pre-flight produced an interrupted verdict with zero tasks executed. Second, the staticcheck negative-control recipe that worked: write a scratch file containing an undefined symbol and expect exit 1 naming the file on both prod and `.test` package type-check paths; remove it; then a scoped `staticcheck ./pkg/... ./pkg/.../internal/` exit 0 stands as landing evidence, with full-tree findings attributed via `git diff HEAD --stat` on the offending subpackage (empty diff = pre-existing baseline). Load-shape doctrine and the substitution ladder belong to the gate's own documentation; bounded and detached wait patterns: <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::harness/reference/gate_runner_pattern.md" />.

## Watchers target recorded PIDs, never patterns

A gate-watcher built as `pgrep -f '<pattern>' | head -1` can latch onto a PEER's identical-shaped command — dozens of concurrent validation agents run the same gate shapes, so the first pid match is a lottery. Observed 2026-09-24: a watcher burned its full nine-minute window on a stranger's gate because the pattern `<gate>-linu[x]_arm64 check` matched the peer's shell eval line first; the `[x]` bracket trick only prevents self-matches and cannot distinguish your run from a peer running the same tool.

Resolve the PID once from a pattern unique to YOUR argument set (the full `--path` list, not the binary name), echo it, then `tail --pid=<exact pid> -f /dev/null` inside a bounded `timeout`. Verify liveness with `/proc/<pid>/stat` CPU-tick deltas (utime+stime, fields 14+15): ~8% of a core means starving-but-working (grant one more window); 0 means dead-wait (kill). Under a ~25-process peer fleet every gate shape starved — full, compile+lint+format, compile alone, even compile with config resolution bypassed — while `staticcheck` over the full closure exited fast on the fleet's warm module caches: the substitute compile-evidence path when gates starve.

## sed prints multi-address output in file order

`sed -n 'A p;B p;C p'` with non-ascending addresses prints in file order, not address order: sed processes the addresses as given but prints as it streams the file, so output is always ascending by position. Matching output lines back to addresses in the order you wrote them silently misattributes evidence — observed 2026-09-24, `sed -n '1100p;1094p;1016p;963p'` output mapped in written order exactly inverted two exemption decisions (two non-pub fn docs skipped that needed rewrites; two exempt pub-field docs rewritten), caught only because the final map still listed the "skipped" runs.

List addresses in ascending order, or better, verify each line by reading the actual region around it; the discipline applies to any multi-address sed/awk used as verification evidence.

Side arithmetic from the same task, for map-style comment checkers that flag runs of `^\s*//` lines over 5 lines or 250 stripped-total chars: with `// ` prefixes a 5-line run allows ~235 content chars, `/// ` and `//! ` about 230 — a textwrap helper asserting <=248 with no prefix is looser than the real budget, so keep condensed text <=225 and let the checker map be the arbiter.

## Report paths and verification channels corrupt under concurrent writers

During a git-commit round, three sequential write/verify attempts of the same report corrupted in three different ways: a bash heredoc write landed mangled (injected repetitions, paths that exist in no porcelain capture anywhere); a Read of a freshly generated /tmp block file showed invented paths; an in-process `comm` verification returned impossible numbers (live=83 against a thrice-proven 87). The only stable source was the FIRST in-context capture, cross-corroborated by independent tool results (diffstat, `wc -l` of new files, rg output) — a host's shims can each corrupt a different slice, so verifying through any one channel is insufficient and a chain of them compounds the damage.

When writing path-exact deliverables (commit reports, work-item lists): transcribe lists from the first verified capture in context, not from re-reads; corroborate each path through at least two independent tool results before trusting it; embed a mechanical re-derivation one-liner (`git status` + cut) in the report so the executor never trusts the literal list; and when a verification channel contradicts a proven fact, stop the verification loop and document the hazard instead of chasing phantom mismatches. Round mechanics: the repository's own commit workflow.

## Untracked plan files vanish before execution

A commit plan that includes untracked files must re-verify every untracked path for existence at validation/execution time: `git status` shows a peer's deletion of an untracked file as nothing at all — the path simply stops appearing, with no commit and no git trace. Observed 2026-09-10: between plan and validation a peer deleted all three untracked members of one strand (a feature file, a probe test, a negative-control canary), replaced the probe strand with two different test files, and landed two commits consuming other planned paths; a validation that only diffed plan-versus-porcelain as "no longer dirty = superseded" would have silently passed over a vanished feature strand and recommended `git add`-ing nonexistent paths.

At validation, partition each planned path by porcelain status: tracked-dirty (may be superseded — verify via the landing commit's diff-stat), untracked (may be gone — `ls`/read it; deletion is invisible), absent-from-status (check whether a landing commit consumed it or a peer deleted it). New unplanned untracked test files adjacent to a revised commit are fold-in candidates when they test that commit's code.

## A refuted premise does not decide a record-only close

When a work issue carries a binding premise correction from a predecessor — e.g. "stale comment" where the comment's core claim was confirmed — the close decision is not automatic record-only. Ask whether the artifact leaves a reader with a wrong model of the current state: an omission that materially misleads (a mechanism that now exists but is never mentioned; an outcome a future auditor will re-litigate) justifies an EDIT close that preserves the confirmed claims and adds only what was missing. Observed 2026-09-06: the refutation identified two real gaps; editing closed the issue permanently instead of deferring re-derivation.

Read the binding handoff, then re-verify every decisive code anchor yourself before choosing the close; write the accuracy proof into the issue annotations either way; cite symbols, not line numbers, in rewritten comments; never put work-item IDs in committed content (a work-reference guard may veto them, and they rot). Premises as snapshots: `stale_inputs_and_drift` in the ctxmod builtin `delegation` profile.
