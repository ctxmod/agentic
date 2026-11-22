# Edit-Verification Protocol — Write-Drift and the Fuzzy Verifier

Verifying written deliverables on a host that exhibits write drift: Write-tool content can land letter-drifted — identifiers mutated into plausible-but-wrong spellings and whole lines silently dropped — so read-back inspection can never arbitrate a write; the protocol is a filesystem-enumerated Python verifier that fuzzy-matches report tokens against ground truth at difflib cutoff ~0.82 and checks structural tallies, trusting only its boolean output.

Content written via the Write tool can, on an affected host, land letter-drifted:
a report that "wrote successfully" may carry mutated identifiers (test
names, filenames, function tokens that read back as plausible-but-wrong
spellings) and silently DROPPED lines — during one test-analysis sweep,
one full table row vanished from a
findings report and every drifted token survived three read-back
inspections.

## Why read-back verification is void there

Read output drifts the same way the write did, and the eye normalizes
plausible spellings, so re-reading a deliverable proves nothing about
its bytes. This extends the host's known output-mutation family — the
Bash-echo corruption that mangles long identifier lists
(<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::harness/reference/harness_traps.md" />)
— to tool-authored file content itself: the corruption is not confined
to echoed command output.

## The protocol: enumerate, fuzzy-match, tally

Never verify a generated report by re-reading it. Write a small Python
verifier (via the Write tool), run it once, and trust its boolean
output — Python-side comparisons are byte-exact even while echoed
output mutates, because the interpreter reads the file directly.

1. **Enumerate ground truth from the filesystem**, not from the
   report: `os.listdir` over the target directories and regex
   `func Test\w+` over the real source files, so the verifier's
   reference set cannot inherit the drift it is hunting.
2. **Fuzzy-match report tokens against ground truth with `difflib`**
   at cutoff **~0.82** — the threshold calibrated on the affected host
   (recalibrate on another).
   Near-misses are replaced with the real spelling; references that
   resolve to nothing above the cutoff are flagged as unresolvable
   instead of being accepted or silently dropped.
3. **Check structural tallies**: table-row counts against expected
   cardinalities and tally consistency across sections. This is the
   only check that catches the silently-dropped-row failure mode —
   token matching cannot see it, because a missing row leaves no
   drifted token behind.

## Verifier shape

Two files: a normalizer (spell-corrects report tokens against ground
truth; emits the resolved mapping and the unresolved residue) and a
structural checker (row counts, tallies, unresolvable-ref exit
verdict). Keep them under /tmp — they vanish between sessions, so
treat each as single-use: regenerate per verification rather than
hoarding. Verdicts produced this way are verified evidence in the
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/verification_evidence.md" /> sense — a
mechanically derived boolean, not a narrative claim.

<!-- anchor: host-write-drift-fuzzy-verification -->

## Related

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::harness/reference/harness_traps.md" /> — the Edit-echo corruption
  family and the Read phantom-tail line; a scoped compile is the only
  trustworthy Edit-corruption detector, and this protocol is the
  report-shaped counterpart for generated deliverables.
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/verification_evidence.md" /> — verdict vs
  narrative evidence discipline for gate and runner output.
- The host's own fabricated-evidence canon (git shims, ls shim, /proc
  scans) in its private context — what the fuzzy verifier's filesystem
  enumeration sidesteps.
