# Decision-Map Planning Objective

Plan an oversized, decision-fogged effort as a map of decision work
items: an effort too large for one agent session to hold, whose way
forward is hidden behind open questions. The map finds the way, it
does not walk it — each item on the map resolves a decision, and the
map is done when nothing is left to decide before execution starts.

This variant is additive to the base planning objective and the other
variants, not a replacement: it defines the item vocabulary and the
working rhythm for fogged efforts; every other planning behavior
still applies.

## Destination First

Name the destination before charting anything else: what reaching the
end of this effort looks like — a decision to lock, an intent
document ready to hand off, a change made in place. One or two lines,
at the top of the epic. The destination fixes the scope: every item
on the map either moves toward it or belongs out of scope, and the
pull to start executing is usually the signal that the edge of the
map has been reached and it is time to hand off.

## Decisions, Not Deliverables

Each child work item of the epic resolves a decision — a question
whose answer is a choice — not a slice of a build to execute. Default
to plan-don't-do: produce decisions, not deliverables. An effort can
carry execution inside the map, but only when its epic says so
explicitly; absent that, resolving decisions is the whole job.

## The Epic Is an Index, Not a Store

The epic is the map; its body is the whole map at low resolution. It
is an index, not a store: each decision lives in exactly one work
item, and the epic never restates it. Decisions made so far are
listed one line per completed item — the item's title, a pointer, a
one-line gist of the answer — linking out for detail. Two other body
lists keep the rest of the view: the not-yet-specified list (fog,
below) and the out-of-scope list (below).

## Blocking Edges and the Frontier

Every decision item declares its blocking edges — the items whose
decisions gate it — in the item record's `blocked_by` field (an
unmanaged field set with `ctxmod work set`). An item is
takeable when every item blocking it is complete; the takeable set is
the working frontier, the edge of what is currently decidable.

Claim before work: create the item record (`ctxmod work new`), write
it, and claim it (`ctxmod work claim <id>`) before starting on it, so
a concurrent session working the same frontier sees the claim. The
store's claim (the record's session assignee) is the only claim
record — there are no human assignee names, labels, or a
parallel-session protocol to coordinate. One decision item per
session, start to resolution.

`reserve_before_create` in the ctxmod builtin `work_discipline` profile

## Fog of War

The map is deliberately incomplete: do not chart what cannot yet be
seen. Beyond the live items lies the fog — decisions and
investigations clearly coming, but not yet sharp enough to state,
because they hang on decisions still open. Fog is written down in the
epic's not-yet-specified list: in scope, just dim. Resolving an item
clears the fog ahead of it; graduate whatever has become specifiable
into fresh items, and remove each graduated patch from the list so it
lives only as its item.

**The work-item-when-sharp test**: promote a patch of fog into a
work item when its question can be stated precisely now — not when it
can be answered now. A sharp question that is blocked is already an
item, carrying `blockedBy` edges and no work done; a question that
cannot yet be phrased that sharply stays fog. Do not pre-slice fog
into item-sized pieces: one patch may graduate into several items, or
into none, once the frontier reaches it.

## Out of Scope

Fog gathers toward the destination only. Work consciously ruled to
lie beyond it is out of scope: it is not fog, does not belong in the
not-yet-specified list, and never graduates — the frontier stops at
the destination. It gets one line in the epic's out-of-scope list:
the gist and why it is out of scope.

When an item already on the map turns out to sit past the
destination, close it and leave that line; a closed item is
unambiguously off the frontier. Scope boundaries are not steps on the
route, so closed items stay out of the decisions-so-far record.

## Refer by Name

In everything a human reads — narration, status, the epic's body —
refer to items by title, never by a bare identifier. A wall of bare
IDs is illegible; names read at a glance. Identifiers and paths do
not vanish: a name wraps its reference, and they ride inside the
name, never stand in for it.
