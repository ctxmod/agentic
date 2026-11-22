# HEAD Baselines and Comparison Mechanics

Proving failures or findings pre-existing: HEAD baseline trees via git clone or git-archive plus fresh git init (both sanctioned, stash banned), frozen-snapshot construction and execution mechanics, byte-stability comparison, and measurement traps that fabricate clean results.

Never assert "these findings/failures are pre-existing" without proof:
run the identical scoped invocation against a clean HEAD tree and diff
the result sets. `git stash` for baselines is banned (it has wiped
uncommitted work in a shared repository).

## A pre-existing proof bounds attribution, not repair

The HEAD-baseline comparison answers one question — did this failure
exist before my change? — and the answer never discharges repair for
touched scope. Failures in touched plus demonstrably related packages
are fixed by the story-completion validator (Haiku-class, acting
validator contract) rather than deferred as baseline: including a proven
pre-existing failure, which is never left broken as "baseline". A
failure proven to come solely from an untouched, unrelated scope is the
only deferral case — it is recorded without expanding the commit and
does not trigger whole-tree cleanup. The untouched-scope attribution
below is therefore a narrow carve-out, not a general deferral license;
the validator contract is in
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />.

## The baseline vehicles: `git clone` or `git archive` + fresh `git init`

Doctrine status: archive-plus-init-sanctioned (operator adjudication
2026-09-27). Both vehicles below are sanctioned and interchangeable —
pick by purpose, not by doctrine.

**Comprehension baselines** — proving a finding or failure pre-existing
by re-running the identical scoped invocation on a clean HEAD tree:

```
git clone --quiet --local --no-hardlinks <repo> /tmp/baseline
# (or --shared; committed HEAD only — uncommitted changes excluded, which is the point)
```

Then run the identical scoped check-gate invocation (same path prefix,
structured JSON output) inside the clone and diff the flattened issue
list (or test names) against the working tree's run. The clone stays the canonical
first choice here: one command, full git metadata, no staging steps.

**Gate-execution and measurement snapshots** — pinned-era binary
reproduction, wall-pair benchmark legs, and gate-bypass runs when the
live repo's facade is wedged. The frozen-snapshot recipe is equally
sanctioned:

```
git archive HEAD | tar -x -C <snap>   # snapshot placement: see below
cd <snap> && git init
# stage explicit top-level paths (broad-add guard, below), then commit
# build the tool under test from <snap> into a separate build dir
```

The fresh `git init` restores the git metadata whose absence motivated
the original void ruling — init alone clears a check gate's
"repository not found" refusal; the
committed snapshot tree is additionally what makes `go version -m`
stamp `vcs.revision` and `vcs.modified=false`, so commit whenever the
snapshot feeds a measurement leg.

**A bare `git archive` extraction WITHOUT `git init` remains void.**
The tree has no `.git`, a repository-aware check gate refuses it
("repository not found"), and that surfaces as `status: failed` with
zero test errors — a silent masquerade as a "baseline failure". Always read the
`message` field of a baseline run, not just the status. The earlier
absolute — "`git archive HEAD | tar -x` is methodologically void" —
was correct about this metadata-less form; it survives as historical
context and as the exact boundary the 2026-09-27 amendment draws.

Archive extraction also chases the module closure (replace directives,
third_party) dependency by dependency.

- **Where worktrees are banned for agents** and `git worktree add` is
  guard-blocked (same block list as `git reset`/`git stash`), the two
  recipes above are the vehicles;
  `worktree_baselines_and_isolation` in the ctxmod builtin `worktree_hygiene` profile
  carries the ban and the same supersession.
- Historical recipe note: when `.golangci.yml`, `go.sum`, and `embed.go`
  were gitignored, a baseline tree needed copies of all three or the
  lint finding set was wrong (default-linter fallback: 19 issues vs 7
  with the real config) and whole trees failed typecheck. These are
  tracked now, but verify per-repo before assuming a fresh snapshot is
  lint-complete.

For lint baselines, diff findings per linter between the HEAD run and
the working-tree run; zero working-tree-only findings proves the fix
introduced nothing.

## Frozen-snapshot staging: the broad-add guard

Where a broad-add PreToolUse guard is installed, `git add -A` /
`git add .` is denied **even inside a scratch snapshot repo** — the
guard pattern-matches the command shape, not the repo, and the denial
kills the whole compound before anything runs. Stage instead by literal
top-level paths enumerated with `ls -A` (`git add .cargo .config
context docs examples .github pkg tools Cargo.lock ...`), then `git add -f` the gitignored-but-required files (a plain
add silently refuses files matching the repo's own `.gitignore` even
when they are tracked in the live repo), and commit with
`git -c user.email=agent@example.invalid -c user.name=agent`. `git status
--porcelain` still reads 0 lines — ignored files are status-invisible,
so a clean status never proves the staging captured them. The Bash
guard also rejects `$VAR` in command position: use literal absolute
paths in leg commands.

<!-- anchor: snapshot-recipe-git-add-guard -->

## Frozen-snapshot execution mechanics

Before any measurement leg, verify binary provenance: `go version -m`
must stamp `vcs.revision == <snapshot HEAD>` AND `vcs.modified=false` —
that requires the committed snapshot tree. `/tmp` evicts snapshots
between calls: never reuse a stale snapshot or binary; re-derive and
re-verify the sha before and after each leg, and place consequential
snapshots in a persistent scratch location outside any bind-mounted
checkout tree (bind-mount alias spellings can split a tool's view of
one store). Invoke the frozen binary by absolute path, unwrapped (no
output-rewriting proxy or shim), with cwd set to the snapshot dir;
capture exit codes with `EXIT=$?` unpiped; run legs foreground, one at
a time, control first, and label load windows from `/proc/loadavg`
before and after. The first leg per snapshot pays the gate's cold
cache settle (an order of magnitude slower than warm in one
measurement) — account for it in any A/B wall pair or the first
comparison is cache-tilted.

<!-- anchor: frozen-snapshot-derivation-mechanics -->

## Byte-stability comparison gates

- **A `diff` shim or wrapper can lie**: one has reported "Files are
  identical" for files with different sha256 values. Gate byte equality
  on `sha256sum` or a Python comparison — never a bare `diff` exit code.
- **A gate run that targets one test can re-execute sibling harnesses
  in the package** and clobber `/tmp` artifacts a baseline manifest
  wrote. The moment a baseline capture succeeds, copy it to a `.frozen`
  path and hash that copy; never re-read the live manifest path after a
  later gate invocation.
- **Generated output that tracks live-repo churn** (for example a
  generated ignore file that emits one line per classified file in the
  live tree) changes between captures with any peer commit. For a
  byte-stability gate, compare the churn-stable subset pre/post, and
  prove no drift with a same-run four-way identity (warm, cold, warm
  re-run, cold re-run all hash equal, the churn-tracking output
  included); the double-run against unmodified code is the control that
  proves churn is not swap-attributable.
- **warm == cold cannot prove a source swap.** When migrating a data
  source where both views carry identical data, every warm/cold equality
  passes even if a call site silently reverts to the old source. The
  only assertion that proves the source moved feeds a deliberately
  mutated data set through the new path and asserts the mutation
  reaches the output — then negative-controls that test.

## Measurement traps that fabricate clean results

- Never verify a test run through `go test ./... | grep ... | head -N`.
  `head` closes the pipe early, the run backgrounds, and the captured
  output contains only already-written stderr — no FAIL lines, no
  package summaries — while the pipeline's exit code reports 0. Redirect
  to a file first (`go test ... > /tmp/x.log 2>&1; echo "EXIT=$?"`) and
  grep the file.
- The same trap destroys compile evidence: piping build or compile
  output through `head`/`tail` (`go build ./... 2>&1 | head -5`) keeps
  the first lines and discards the diagnostics that justified the step —
  redirect to a file and read it
  (`go build ... > /tmp/b.log 2>&1; echo "EXIT=$?"`), one build per
  Bash call (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/verification_evidence.md" />).
- Distrust a clean result that arrived faster than the suite's known
  runtime. When a local check contradicts a sub-agent's report of
  failures, re-run with file redirection and read the real exit code
  before overriding them.
- When the compile-evidence channel is staticcheck, piping it through
  `tail`/`grep` makes `$?` report the pipe's last stage, not
  staticcheck's — capture with the redirect-plus-`rc=$?` shape and
  classify output by line shape (trailing check code = lint finding on
  a package that still type-checked; any other line = compile
  failure).

## Related

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/negative_controls.md" />
- `worktree_baselines_and_isolation` in the ctxmod builtin `worktree_hygiene` profile
