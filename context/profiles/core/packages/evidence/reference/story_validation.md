# Story Validation — Scope Attribution and AC Location

Mechanics for the validation passes that examine a story before it
closes: how to attribute changed scope when the working tree is
peer-polluted, which clerical repairs a validator may make itself, and
where acceptance criteria live when a story was re-priced mid-flight.
The boundary itself — who validates, when it starts, and the one-session
cycle — is <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />;
this file carries the judgement rules those passes apply to the work
items and the diff in front of them.

## Per-commit scope attribution

Attribute story scope PER-COMMIT, never against the aggregate
`git diff <base>..HEAD`. The fleet runs parallel stories on one main
branch, so the diff between a story's base pin and HEAD always contains
peer work — reading it as the story's delta misattributes every
parallel-chain commit to the story under validation. On one story
(2026-09-15) the aggregate showed ~200 files, including three
story-protected source files, all from an interleaved peer comment
sweep plus parallel-chain commits, while the story's own four commits
were perfectly scoped — the violation existed only in the aggregate
reading (documented in that story's validation-structure report).

The procedure: `git log <base>..HEAD --format='%h %s' -- <declared
scope>` to enumerate who touched the declared scope, then
`git show --stat` each of the story's own commits. Document the
attribution explicitly in the validation report so the later validation
passes (references, criteria-evidence) do not misread the aggregate as
a violation. This is the review-side counterpart of commit-time
scope discipline in the repository's own commit workflow and the ctxmod
builtin `commit_discipline` profile.

## The sanctioned clerical-repair set

A structure-validation pass may repair clerical defects in work items
without escalating — exercised without pushback on that story:

- issue status flips to `done` (`ctxmod work set <id> status=done`) when
  docket-verified
- AC-checkbox alignment to each report's own honest checklist, leaving
  genuinely carried boxes unticked with an in-box carry pointer
- appending landed-report paths to the issue body's related list
  (`ITEM.md`)
- repositioning report frontmatter that landed below the leading H1
  back to byte 0 — check head-of-file position, the defect is easy to
  miss

Never repair code, test files, or landed evidence CONTENT: an exec
docket's orphaned template rows are left in place and flagged, owner
the exec agent. The validator repairs metadata and placement, not
another agent's recorded evidence. Stale issue status fields and
month-directory drift are non-blocking clerical drift, classified as
such in the report rather than repaired. Hand-assembling a report path
is never a fallback: when `ctxmod work new` fails (for example, timing
out on the store lock under peer load), retry it, and surface the error
if the retry fails. The store stays the single source of truth for
id and path minting, exactly as the reserve-before-create rule
prohibits max-ID scans.

<!-- anchor: scope-attribution-and-sanctioned-repair-set -->

## Re-priced-story AC location

When a story is re-priced mid-flight — analysis verdict sections
appended to issues as `## Re-scope <date> (verdict X)` — the story
body's scope/AC text is NOT revised. It keeps the original pre-re-pricing bar,
and no story-level re-scope addendum is added; the binding
re-interpretation lives only in the per-issue re-scope sections plus
the planning report. On one story all four issues carried
`## Re-scope 2026-09-17 (verdict B)` sections while the story's ACs
(numbered 1..4) still described the original ambition — one issue's
"<1s-p50 leg" had been ruled obviated — and the story body was ruled
structurally coherent as-written (the ruling is that story's
validate-structure report).

Only story-level ACs carry numbers; issue-level ACs are plain
unnumbered checkbox bullets. In validation of a re-priced story:

- report the raw AC checkbox state — usually 0 ticked, because the
  completion signal is exec-note plus commit evidence, not the boxes
- hand the criteria-evidence pass the location pointer: the issue
  re-scope sections are the AC interpretation SSOT (that story's
  validate-criteria-evidence report is the worked example)
- read verdict evidence honestly per
  <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/verification_evidence.md" />

The structure pass makes a single write — its validation report — and
never repairs production or work-item bytes; that write is the whole of
its authority.

<!-- anchor: repriced-story-ac-location -->
