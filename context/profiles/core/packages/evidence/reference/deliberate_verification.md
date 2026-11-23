# Deliberate Verification

Verification is deliberate, not automatic: test evidence is produced
only when an agent explicitly chooses and invokes the relevant test
form. No hook, pre-commit, pre-push, or CI path executes any test
automatically. Between phases, agents run risk-based local checks —
cheap, deterministic, in their own tree, scaled to the risk of the
change. One mandatory high-level validation boundary exists: story
completion.

The two deliberate test forms, selectable by any agent for a question
or acceptance criterion:

- package tests — implementation-coupled, quality-claim evidence
  (selected by name and run deliberately through the repository's
  check gate; authoring and record depth live in the repository's own
  test documentation)
- test tools — black-box, working-behavior evidence (programs run live
  against the built binary)

Both forms verify one piece of work. Whether an agent setup — its
model, harness, tools, and loaded context — does the job reliably
across many runs is a different question, answered by evals:
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/agent_evals.md" />.

File-stage quality evidence requires armed policy: the check verdict
record must carry checkable proof that the policy sections were
enforced for the run — in a gate that records it, a structured
`policy_armed` field with a config-resolution state (resolved /
degraded / absent).

<!-- anchor: validators-fail-closed -->
## Validators fail closed on unreachable

A verdict exists only when the verifier ran to completion. Every branch
where a validator is unreachable, a dependency is missing, or an input
is unfetchable exits nonzero and names the reason: a warning, a
degraded check tier, or a partial scan never substitutes for a verdict,
and a gate whose scan infrastructure fails (a grep, parser, or reader
erroring — not merely matching nothing) fails instead of reporting an
empty clean result. The worked counter-example is a 2026-10-04
app-package build (final-audit finding F8): the clean-first-run
schema download failed into the branch that handled "schema
unreachable" by printing one stderr line and exiting 0, and a missing
python jsonschema degraded to a warning plus exit 0 — a "validated"
package was committed before anyone detected that no validation had
run. The remediated shape is the standard: each unreachable branch
exits nonzero with the cause named; a stale deliverable from an earlier
run is removed so nothing can be mistaken for a validated artifact; an
environmental skip is loud and nonzero (a designated skip code such as
77 with the reason on stderr, never 0); a missing-dependency fallback
tier that would end in an "ok" is removed rather than softened; and the
evidence that a validator fails closed is a forced-unreachable harness
— run the validator with the dependency or input deliberately
unreachable in a scratch tree and record the nonzero exits, plus a
reachable-path control so the nonzero assertions are not vacuous.

<!-- anchor: gates-exhaustive -->
## Gates are exhaustive or declare elision

A gate's verdict is a coverage claim. When the findings it reports are a
subset of what the claimed scope produces, the gate is worse than no
gate: the artifact ships through a green run, and a "fix the findings"
loop against the subset can converge forever. The canonical
counter-example is the 2026-10-02 padding-checker commission, where
partial reporting was first read as two source hypotheses (binary versus
wrapper) and the operator's brief re-framed it as the defect itself —
"the gate under-reported, so a 'fix the findings' loop could loop
forever" (finding F-L1-5) — the same class as the finding-collapse bug
that had produced a converge-forever lint loop (fixed live 2026-10-03).
The standing discipline: before trusting a green verdict, confirm the
gate's coverage matches the verdict's claim — same file set, same rule
set, same invocation shape as the consumer of the verdict, because a
local gate that passes where CI fails is a subset gate wearing the same
verdict. When a gate deliberately covers less, the verdict says so: an
elision is declared in the verdict itself ("rules X and Y not run:
binary missing"), never discovered downstream. Adjacent failure: a
validator that cannot run at all fails closed (the previous section); a
validator that runs on a subset is this section's failure — both launder
a green verdict, and a summary line must refuse to report green over
either.

<!-- anchor: working-behavior-smoke -->
## Working-behavior smoke before live claims

A "live/deployed" claim is a working-behavior claim and obeys the claim
pair: it is established only by deliberately executing the deployed
artifact's primary journey, never by build success plus endpoint 200s.
The smoke scope is the journey users take, asserted in a real or
headless browser. The 2026-10-04 "live" verdict (HTTP/2 200 from the SPA
shell and the config endpoint, certificate verified) was contradicted 22
minutes later by a 401 on the authenticated primary call; the next day
the published site reached users with the UI provider library unmounted
(providerCount=0, computed fontFamily Times New Roman) because the check
compared structure — and jsdom in fact reads the provider's CSSOM and
computed custom properties, so structural-only checks are a chosen
weakness, not a necessity. The remediation set the durable shape: a
regression gate that "drives the production entry point itself, not a
test-only provider tree". Supporting evidence of genuine execution rides
along: a gate records its duration or equivalent execution proof (the
genuine 30.56s formatter leg), because a zero-second pass is a skip in
green clothing, and a fresh-binary re-verification excludes stale-binary
false results.

<!-- anchor: validation-dispatch -->
## Compaction-resistant validation dispatch

A validation dispatch is designed to survive context continuation,
because continuation is the expected case, not an anomaly: the
2026-10-06 validation rounds recorded an 18-compaction read-only
validator (a 26.4 MB transcript that was mostly repeated compaction
summaries), 13 compactions across ten files in one group, and 13+
harness context prunes in the coordinating session, each re-injecting
an 18–22 KB summary as a user-role line. Five disciplines make the
dispatch continuation-proof:

- **Coordination tokens in briefs.** Every validation brief carries a
  token naming the gate owner and the expected evidence artifact. A
  compaction continuation's first action is to check its token — before
  any compulsory read — and self-park read-only when another identity
  holds the gate or the token was superseded. The W4 round's parked
  duplicate did stop before any oracle ("the compaction continuation
  made me a duplicate of the already-running independent W4 validator…
  The reserved artifact was never written and must not be treated as
  evidence"), but only after reading four compulsory documents; the
  token check is what makes parking cheap, because the reads are what
  compaction punishes.
- **Incremental evidence artifacts.** Validators write evidence to disk
  as each check completes — the EXITCODES.txt pattern: an append-only
  evidence file opened at dispatch start, one line per check (name,
  exit code, timestamp) — so a compaction at any moment leaves durable
  partial evidence. The report or verdict is a summary of the artifact,
  written last; it is never the sole carrier of the evidence.
- **Narrow scope.** One gate or surface per dispatch. The
  18-compaction validator was wide-scope read-only validation; scope
  width is the compaction multiplier.
- **Changes queue to check boundaries.** A mandate change mid-run — a
  scope re-pricing, a correction relay — is queued until the current
  check, or the validation itself, completes. Mid-run injection
  re-priced one read-only validator's scope repeatedly and fed the
  compaction load.
- **Full-form hashes and post-continuation re-verification.** Hashes
  and ids in evidence are written in full form or as a verified
  reference, never truncated: two sha values in one round's evidence
  were written truncated from pre-compaction notes — evidence
  corruption by summarization. Every continuation re-verifies prior
  evidence against live state before building on it; the truncated-sha
  damage was caught exactly this way, and the re-verification is
  mandatory, not lucky.

When report writes are prohibited, findings persist through the
sanctioned findings channel
(<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/findings_channel.md" />),
not through /tmp files or return-message prose.

<!-- anchor: pipe-exit-evidence -->
## Pipe-exit-code evidence discipline

A pipeline reports its tail's exit status, so `$?` observed after
`cmd | tail` or `cmd | head` is evidence about the shaper, never about
the piped command. Verdicts come from unpiped re-runs: a gate or
install whose output was piped for display is re-run unpiped before any
pass/fail claim, and evidence JSON records the literal exit code of the
unpiped command (the findings channel's evidence schema), never a
pipe-shaped status. The U27 self-catch is the canonical case: the
reported "the check gate exits 0 on error" bug never existed — "the exit 0 came
from the pipe" — and the unpiped re-run showed exit 1; the same
campaign recorded an install announcing INSTALL_EXIT=0 while it had
failed closed ("I piped through tail, so $? was tail's") and a
pipe-swallowed rc beside a shim misread (an output-rewriting wrapper
presenting Biome output as "ESLint"). A PreToolUse guard backs the
doctrine mechanically (the ctxmod builtin `shell_hygiene` profile ships
one as its pipe-tail exit-status hook): a pipeline-final `tail`/`head` capture pattern is flagged
at warning level with the unpiped substitute — an advice the command is
allowed under, never a block.

<!-- anchor: disposition-ledger -->
## Verdicts end with a disposition ledger

A validation verdict is finished only when every finding it states
carries a disposition, because verdicts that stop at recommendations
let correct findings decay: the campaign's final-audit findings — F5
(act deploys unbuilt bytes), F7 (the SPA shipped without a
user-visible error state), F8 (schema download before mkdir), and F9
(citation presence proves nothing) — were recorded as "recommendations
only; none were applied" and had zero remediation traffic by session
end. The ledger is the verdict's closing artifact: one row per
finding, each naming its disposition (acted or deferred — the findings
channel's status enum), the owning agent or story, and, for deferrals,
the deferred-until date. An open or unowned row is a defect in the
verdict itself, not a property of the code under validation.

Three obligations make the ledger load-bearing:

- **Interim mitigation on enforcement-weakening deferrals.** A
  deferral that weakens enforcement — a check skipped, a guard left
  on-demand, a freshness check deferred — names, in its own row, the
  interim mitigation holding the risk down until the item lands: a
  startup warning, a scheduled re-check, a manual gate — anything is
  acceptable except the bare deferral. The counter-examples are
  one story's deferred freshness AC, which became a measured live hook
  bypass within four days, and the guard-completeness check left
  on-demand ("the check only runs when someone runs it on purpose") —
  both tracked as backlog pointers while the weakened enforcement ran
  unprotected.
- **The scheduled sweep.** The ledger is consumed, not only written.
  The trigger is the story-completion boundary: before a story closes,
  its story-completion validation sweeps the disposition rows its
  verdicts own — a deferred-until that has lapsed without action
  re-opens the row (the deferral has expired and the finding is live
  again), and epic completion sweeps the epic's full table. A sweep
  that finds nothing due records that fact; an unswept ledger is the
  failure mode this doctrine exists for.
- **The closure escape.** Items without dispositions block closure:
  the coordinator re-dispatches each undispositioned finding to an
  owner or records an explicit waiver — which is itself a disposition
  row with an owner and an until date, never silence.

Worked example — the F5/F7/F8/F9 final-audit findings walked through
the table (a marked sketch reconstructed from the incident record:
owners and dates are placeholders, and only F8's row reflects a real
remediation — its class was re-raised into the validators-fail-closed
doctrine after the session ended; at audit close all four rows were
open and blocking):

| Finding | Disposition | Owner/story | Deferred-until | Interim mitigation |
| --- | --- | --- | --- | --- |
| F5 — act deploys unbuilt bytes | deferred | writer (demo workflows) | 2026-10-10 | act paths gated so local runs never deploy bytes they did not build |
| F7 — SPA without a user-visible error state | deferred | writer (demo SPA) | 2026-10-12 | fail-closed release summary labels skipped legs instead of passing green |
| F8 — schema download before mkdir | acted | validator | — | n/a — the class landed as the validators-fail-closed doctrine |
| F9 — citation presence proves nothing | open | — | — | blocks closure: no owner, no disposition |

The F9 row is the escape in action: an unowned, undispositioned
finding cannot ride a green verdict, so the story stays open until the
row resolves. A verdict recorded from a piped run enters the table as
`open` with the unpiped re-run as its acted state (the pipe-exit-code
discipline above), and the table consumes the findings channel's
`status` enum so a findings-evidence file sweeps without re-deriving
disposition.

## The story-completion boundary

Each story closes through a Haiku-class story-completion validator
operating under the acting validator contract. It derives touched and
related scope, runs deterministic scoped checks, repairs failures
in-session, rechecks every changed byte, and commits with commit-scope
byte identity. It starts after all implementation work for the story
is ready — never after each file edit, issue substep, or trivial
correction. No other sub-agent holds this authority, and no per-phase
validator fan-out substitutes for it; the iteration pattern behind the
cycle is <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />.

## The one-session cycle

The validator completes validate, fix, recheck, and commit in ONE
session; it never hands a fix or commit step to another agent.

- **Input**: story ID, intended changed paths, prior agent handoff,
  acceptance criteria, relevant package-test and test-tool selectors, and
  known unrelated dirty paths.
- **Scope derivation**: touched plus demonstrably related packages —
  derived from imports, generated artifacts, configuration ownership,
  or explicit story scope, recording why each additional path is
  related.
- **Check selection**: scoped check-gate runs appropriate to the changed
  artifacts, plus deliberately selected package tests and test tools only
  when they answer a story acceptance criterion or material risk.
- **Repair**: any failure in that selected scope is fixed, including
  a proven pre-existing failure, which is never left broken as
  "baseline".
- **Untouched failures**: a failure proven to come solely from an
  untouched, unrelated scope is recorded without expanding the commit.
- **Recheck**: every changed byte after the first validation pass is
  included in a later successful deterministic check before commit.

If the cycle cannot complete, the validator returns a blocker and
creates no success claim.

## Commit-scope byte identity

The committed bytes must equal the bytes the story validator approved.
Whole-tree cleanliness is not the gate: peer work stays out by scope —
a pre-stage manifest of the exact story path set plus content
identities, path-specific staging (never a bare `git add -A` /
`git add .`), peer pre-staged exclusion, unrelated-dirty tolerance,
and a post-commit proof that the committed versions match the
validated set. Host mechanics and failure modes live with the
repository's own commit workflow and the ctxmod builtin
`commit_discipline` profile. How a validation pass
attributes reviewed scope when the aggregate diff is peer-polluted —
and which clerical repairs it may make itself — is
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/story_validation.md" />.

## The success record

The closing record carries exactly this enumeration: commit
identifier, scoped check-gate result, deliberately run package-test
result set, deliberately run test-tool result set, commit-byte proof.

The conclusion obeys the claim pair: if no relevant test tool was
deliberately exercised, the validator may conclude that scoped quality
gates pass but must not conclude that real-world behavior works. How to
read a check-gate verdict honestly — what a green run's payload actually
carries, and which partial verdicts stay trustworthy — is
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/verification_evidence.md" />.

Which facts from a session deserve recording at all — durable
decisions versus transient observations — is settled in
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/semantic_vs_invariant.md" />.
