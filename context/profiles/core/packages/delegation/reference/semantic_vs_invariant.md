# Semantic Decisions vs Mechanical Invariants

What deserves judgment versus what is mechanically enforced: the semantic-decision / mechanical-invariant split, and the durable-decision / transient-observation distinction that decides what gets recorded.

Semantic choices stay with the agent; mechanical invariants belong to
deterministic mechanisms. Every obligation in this repo sorts into one
of the two, and mixing them is the failure mode: judgment enforced by
machine produces false confidence, and rules left to judgment drift.

## Semantic decisions

Judgment calls an agent makes and is accountable for: which tests to
deliberately select for a claim, why a path counts as related scope,
whether an observational scenario is more honest than a fabricated
assertion, what a comment must say, what a diagnostic should point at.
These need recorded rationale — a reviewer or a later session must be
able to ask "why this choice?" and get an answer.

## Mechanical invariants

Deterministic rules a mechanism enforces identically every run:
comment maxima, conventional-test policy severity, commit-scope byte
identity, the requirement that a green stage include actual selected
records. These are never a judgment call — an agent does not wave a
mechanical invariant through, and a mechanism does not soften one. An
obligation that cannot be stated deterministically is not ready to be
mechanical: it stays semantic until it can be written as a checkable
rule.

## Durable decisions vs transient observations

The recording-side counterpart. Durable decisions are rehydration
state — a decision written into source, the work store, or the context module's `reference/`
survives and lets a fresh session resume work. Transient observations
— raw command output, large scans, intermediate dumps — are disposable
once their decision-relevant result is recorded. Record the result,
not the transcript. The test of durability: would a future session act
differently because this fact exists? If yes, it is a decision and
belongs somewhere durable; if no, it was an observation and can be
dropped.

The story-completion validator applies this split when writing its
success record — see
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />.
Comment maxima as the mechanical side of comment placement:
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/comment_placement.md`.
