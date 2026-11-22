# Review Smell Baseline

Judgement-call baseline for the Standards review axis: twelve code
smells (after Fowler, *Refactoring*, ch. 3) applied to a changeset even
when the repo documents no standards of its own. Documented standards
live in the repository's context-module rules and discovered repo conventions.

## Binding Rules

- **The repo overrides.** A documented repo standard always wins; where
  it endorses something this baseline would flag, suppress the smell.
- **Always a judgement call.** Each smell is a labelled heuristic
  ("possible Feature Envy"), never a hard violation.
- **Skip tooling-enforced checks.** Anything a formatter, linter, or
  typechecker already enforces is out of scope — smells are the
  semantic layer beneath mechanical invariants, not a second linter.

The semantic-versus-mechanical split that places smells on the
judgement side:
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/semantic_vs_invariant.md" />.

## The Twelve Smells

Each entry reads *what it is* → *how to fix*; match it against the
changeset under review:

- **Mysterious Name** — a function, variable, or type whose name does
  not reveal what it does or holds. → Rename it; if no honest name
  comes, the design is murky.
- **Duplicated Code** — the same logic shape appears in more than one
  hunk or file of the change. → Extract the shared shape and call it
  from both sites.
- **Feature Envy** — a method reaches into another type's data more
  than its own. → Move the method onto the data it envies.
- **Data Clumps** — the same few fields or parameters keep travelling
  together (a type wanting to be born). → Bundle them into one type
  and pass that.
- **Primitive Obsession** — a primitive or string standing in for a
  domain concept that deserves its own type. → Give the concept its
  own small type.
- **Repeated Switches** — the same switch or if-cascade on the same
  type recurs across the change. → Replace with polymorphism, or one
  shared map both sites use.
- **Shotgun Surgery** — one logical change forces scattered edits
  across many files of the change. → Gather what changes together
  into one module.
- **Divergent Change** — one file or module is edited for several
  unrelated reasons. → Split it so each module changes for one reason.
- **Speculative Generality** — abstraction, parameters, or hooks added
  for needs no work item has. → Delete it; inline back until a real
  need shows.
- **Message Chains** — long `a.b().c().d()` navigation the caller
  should not depend on. → Hide the walk behind one method on the
  first object.
- **Middle Man** — a class or function that mostly delegates onward.
  → Cut it and call the real target directly.
- **Refused Bequest** — a subtype that ignores or overrides most of
  what it inherits. → Drop the inheritance; use composition.

Baseline smells are reported as labelled judgement calls, cited against
the hunk that shows them — the review-phase skills summarize the
binding rules inline and link here for the catalog.
