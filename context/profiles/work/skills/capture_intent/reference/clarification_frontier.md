# Clarification Frontier

Clarification doctrine for intent capture — the design tree, frontier rounds, the facts-versus-decisions split, and the empty-frontier termination test.

Clarification (Phase 4 of intent capture) works a tree, not a flat
list. Every decision the intent depends on branches into the
decisions that hang off it: settling one decision answers some
questions and exposes others. The **design tree** is the shape of
what is still unknown. A flat list of gaps hides that structure — it
cannot tell a question whose answer unlocks ten others from one that
unlocks nothing.

## The Frontier

The **frontier** is every question whose prerequisites are already
settled: the questions that can be asked now without guessing at
answers not yet heard. A question belongs to the frontier only when
every decision it depends on has been answered; a question whose
answer depends on another question still open in this round belongs
to a later round.

Work the frontier in rounds. Each round asks the askable questions —
numbered, each with a recommended answer — then waits for the user's
answers before the next round. The answers reshape the tree: settled
decisions push the frontier outward and unblock the questions that
hung off them. Recompute the frontier and ask the next round. When
the frontier is larger than one round's frame, ask what unblocks the
most downstream branches first; the rest of the frontier stays
queued, not forgotten.

## Facts Versus Decisions

Finding facts is the agent's job, never the user's. When a frontier
question needs a fact from the environment — a file's contents, a
tool's behavior, an API's response — dispatch a sub-agent to look it
up rather than spending a user question on anything that can be
found. Dispatch non-blocking: a running lookup is an unsettled
prerequisite, so only the questions downstream of it wait for the
sub-agent's report; ask the rest of the frontier now.

The decisions are the user's: put each to them with concrete options
and wait for the answer. The ordering is inference, then lookup, then
the user — the pipeline's "infer, don't ask" rule first, dispatch
second, and the user only for what can be neither inferred from
context nor found by a sub-agent.

## Termination

The session is done when the frontier is empty: every branch of the
design tree visited, nothing left silently assumed. Empty is a
computed result, not an impression — re-walk the tree before
declaring it: every settled decision's children are themselves
settled or asked, and every question is answered or explicitly
carried into the intent's open questions as truly undecidable. Do
not act on the captured intent before the user confirms the shared
understanding it records.

## Decisions of Lasting Architectural Weight

A clarification round occasionally settles a decision whose weight
reaches beyond this intent — a choice later efforts will treat as
fixed. Record it as a numbered confirmed decision in the intent, and
flag it for the repository's decision-record discipline, which owns
how lasting decisions are numbered, dated, superseded, and kept
machine-readable:

`ctx:github.com/ctxmod/engineering/context::shared::structure@repositories/reference/adr_discipline.md`
