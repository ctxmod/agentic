# End-to-End Validation for Visual Output Tools

Process lesson: code-structure validation (checks asserting "the
function exists and is called") can pass while the rendered output is
visually broken. For any tool that produces PDF or visual output, a
working-behavior claim — the real program behaves as users encounter it
— requires deliberate execution of a relevant black-box test tool
against the real binary. Scoped checks and implementation-coupled work
tests alone never establish it. No committed conventional-test suite is
mandated or treated as the target state: the obligation is on the claim,
not on maintaining test artifacts.

## The failure mode

Per-issue validation reports that check code structure will say PASS for
features that do not actually work in the full pipeline. A remediation
that wires a flag, passes the unit test asserting the flag is threaded
through, and reports "done" can still produce broken output because the
unit test never exercised the actual render path.

Concrete examples from rendx: MathJax not wired (flag defaulted to
"off"), `--csl apa` treated as a file path by Kong's `type:"path"`,
compute expressions not evaluated in non-PDF formats, columns stacked
instead of side-by-side, TOC missing in non-PDF outputs, citations raw
in HTML/EPUB. All passed code-structure validation. All were broken in
the rendered PDF.

## Deliberate practices

1. **Exercise actual renders through a black-box test tool** that checks
   the rendered output for expected markers (LaTeX commands, rendered
   text, structural elements). The tool invokes the explicitly resolved
   compiled binary as a subprocess and never imports the production code
   it exercises — a check on the emitter's internals is not end-to-end
   evidence. When the expectation is pinned by a concrete oracle
   (expected marker present, known-good structural property), use
   assertion mode: a failed expectation returns a documented nonzero
   status with expected-versus-observed evidence.

2. **Run the actual binary, not an imported handler.** Binary wiring
   (Kong flags, config resolution, pipeline assembly) is where the
   silent breakage lives — the failure-mode examples above all passed
   code-structure validation while the compiled program was broken.

3. **Include smoke renders per output format.** A minimal scenario
   exercising each output format (PDF, HTML, EPUB, DOCX) catches
   format-specific regressions that per-feature scenarios miss.

4. **Independently verify with `git diff` AND a functional observation.**
   When a worker agent reports "all done," check both that the code
   landed in the working tree and that the rendered output changed in
   the expected way before accepting.

Execution is deliberate: the agent explicitly chooses and invokes the
relevant test tool for the claim being made; no hook, pre-commit,
pre-push, or CI path executes it automatically.

## Observational honesty

When the expected behavior has no concrete oracle yet — visual quality
judged by a human, behavior still being characterized, expected versus
observed not yet pinned — an observational scenario is more honest than
an assertion. Observational mode emits the rendered evidence for agent
or human interpretation: success means the scenario executed and the
evidence was produced, not that the observed behavior was judged
correct. Fabricating an assertion just to get a pass/fail verdict
(one that mirrors today's output rather than the contract) converts
unknown behavior into false confidence. Execution failures still return
nonzero; what observational mode refuses to claim is a correctness
verdict. Aggregate output keeps assertion and observational results
separate and never labels an observational result as passed.

Test-tool authoring and CLI/output contracts live in the repository's
own test-tool documentation; temporary implementation-coupled checks
belong in package tests instead.

## Live binary probes: freshness and probe design

Validating against the actual deployed binary ("run the actual binary",
above) carries two extra obligations — together they are the build-
provenance discipline every deliberate test-tool run owes its claim:

1. **Check the binary is fresh against the code under test.** The
   deployed binary is whatever sits in the build tool's central cache
   (for example `~/.cache/<build-tool>/<tool>-<goos>_<goarch>`), and the
   cache is not rebuilt by merges — only by an explicit build of that
   tool. Before
   live-testing binary behavior, compare the deployed binary's mtime
   (`stat -c '%y'`) against the commit dates being validated; rebuild
   if stale (disclose the artifact change), then re-run the probes. A
   live-surface claim is only as fresh as the binary — a stale binary
   validates pre-change code while the probes "pass".
2. **A suppression/guard probe must use a payload that would produce
   output if the gate were absent** (e.g. `git reset --hard` for the
   command guard). A clean payload (`git status`) is silent both when
   the gate is off and when it behaves correctly — it proves nothing.

## When this applies

Any tool whose output is consumed visually (document renderers, site
builders) or structurally downstream (structured documents a renderer
consumes). The general
principle: the closer the evidence is to the final observable output,
the more regressions it catches. The further it is (mocked pipeline,
asserted struct field), the more it gives false confidence — which is
why the working-behavior claim is reserved for deliberate test-tool
execution against the real binary, and why scoped quality gates passing
never implies it.
