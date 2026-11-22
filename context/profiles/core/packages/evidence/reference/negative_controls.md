# Negative Controls (Mutation Verification)

Mutation-based verification discipline: prove a guard, regression test, or deliberate package-test/test-tool result set is load-bearing by re-introducing the defect — no-vacuous-pass controls (a green result must carry actual selected records), patch/run/restore mechanics, the pass-means-defective-mutation rule, two-sided controls for adapted tests, and the file-surgery quirks of hosts with interactive aliases and wrapped tools.

A regression test or guard is only proven load-bearing by re-introducing
the defect it exists to catch and observing the failure. The same
discipline applies to deliberate verification: a deliberately run
package-test or test-tool result set earns its place in a success record
only when a control can make it fail. Isolation by
`git stash` is banned — it has wiped uncommitted work in a shared repository — and
so is interactive `cp` (it prompts `cp: overwrite '...'?` and hangs until
timeout, leaving the patched file on disk). The sanctioned mechanics are
Python `shutil` plus sha256 verification.

## A green result must carry actual selected records

The no-vacuous-pass rule for deliberate test execution: a green result
is evidence only when its payload contains the records of what actually
ran. Deliberate package-test selection through the check gate must
embed honest-empty-selection semantics — a resolved scope with no
package tests never claims tests passed — and the same suspicion applies
to every other green stage: verify the structured payload (the tests
array / result records in the gate's JSON output), not the exit status,
and confirm the selected package tests appear with outcomes.

Test-tool runs owe the same check. A run that exits green without
executing the selected subtests' payloads — a stubbed binary, a
scenario that short-circuits, a guard probe whose payload would be
silent even with the gate absent — is a defective control, not
evidence. A suppression probe must use a payload that would produce
output if the mechanism under test were absent; then apply the mutation
discipline below to the run itself: neuter the mechanism (an input the
tool must reject, a fixture the scenario must touch) and confirm the
verdict flips before trusting the green.

Authoring and record depth live in the repository's own package-test and test-tool documentation.

## Patch / run / restore mechanics

- Snapshot with `shutil.copy2`; patch with a Python replace guarded by
  `assert old in s` so a drifted anchor fails loudly instead of
  silently no-op'ing.
- Restore via `shutil.copyfile` — never `copy2`/`cp -p` — into a
  same-directory temp file, then `os.replace` within the target
  filesystem. A direct `/tmp`-to-repo `os.replace` fails with
  "Invalid cross-device link".
- Print and verify the sha256 of the restored file in the same command.
- Post-cycle: `git status --porcelain` on the touched paths. When the
  pre-cycle file carried an unrelated peer edit, the backup (not HEAD)
  is the authoritative restore target.
- Token-presence validity checks use the shim-immune fixed-string form:
  `grep -cF "pkg.Call(" file` (count 0 proves removal, 1 proves
  restore). Escaped-paren regex forms can fail under a grep shim or
  wrapper on `PATH`.

**Rust sources:** after restoring, `touch` the file before re-running
gate validation. A check gate that pins `CARGO_TARGET_DIR` per module
lets cargo judge freshness by mtime — a restored mtime older
than the mutated build's fingerprint serves the MUTATED binary
("Failed" three runs in a row after a verified-identical restore). A
direct `cargo test` passing where the gate fails is the two-target-forests
tell, not evidence against the gate.

## A passing control is a claim about the mutation first

"Guard removed, tests still pass" usually means the mutation is
defective, not that the guard is weak. Before reporting a guard as
non-load-bearing, re-read the mutated function in full and confirm no
other branch still produces the same verdict. Prefer short-circuit
mutations (`if true { return <benign> }`) over disabling a single
condition in multi-branch decision functions.

**Multi-guard masking:** when a function applies several guards to the
same candidate, disabling one guard often still passes because a sibling
rejects the same input for a different reason. For each guard, construct
an input every OTHER guard accepts and only the named guard rejects —
pad documents so count-based bounds cannot fire, choose values that are
order-compatible so ordering guards cannot fire. Record the padding
rationale in the test comment so a later reader does not simplify it
away.

**Gate choice:** for a race between child startup statements and a
parent-sent signal, gate the parent on reading the child's own stdout
line, not on a kernel-level observation (connect succeeds at listen
backlog) — kernel-state gates let the child keep a head start, and the
control passes against mutated code even under load.

**Seam reversion:** reverting a construction seam can hand control back
to real host machinery (real service manager instead of a Fake, ~10x
runtime). Re-verify host-side invariants immediately after any control
that touches a seam, not just after the final suite run.

## Adapted tests need TWO controls

When a new guard lands and a pre-existing test is adapted with a bypass
flag, one negative control cannot separate "legitimate reach past an
unrelated guard" from "routes around the guard under test". Run both:

1. Remove only the flag — the test must fail (else the flag was
   gratuitous).
2. Neuter the mechanism the test is named for — the test must fail (else
   the flag made it vacuous).

A legitimate adaptation fails both, and the diff adds only the flag with
every assertion byte-identical. Also verify which branch the fixture
actually takes; the stated mechanism can be wrong even when the
conclusion is right.

## Concurrency and overwrite hazards

A concurrent peer can COMMIT your mutation while the control runs.
Working-tree restore is not restoration verified against HEAD: after
every control, run `git log --oneline -3` and
`git diff HEAD -- <file>`; flag in the handoff if a peer commit landed
mid-window. Run controls in short windows.

Before any rollback-swap or pre-existing-behavior experiment that
overwrites files: `mv` every file the current change created/edited into
a scratch dir first, run the experiment, then move them back and re-run
the package tests. Untracked content destroyed by a snapshot copy-back
is unrecoverable (`git checkout` cannot restore it).

## Related

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/head_baselines.md" />
- `ctx:github.com/ctxmod/agentic/context::shared::harness@shared_tree/reference/harness_traps.md`
