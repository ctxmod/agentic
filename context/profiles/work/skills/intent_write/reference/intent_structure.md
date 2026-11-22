# Intent Document Structure

Template and schema for intent documents — sections, ordering, acceptance criteria quality, scoping guidance, decision documentation, and open-question phrasing.

This reference defines the canonical structure for intent documents
written by `intent_write` (Phase 4 composition) and consumed by
downstream work-item authors, planners, and implementers. It is the
single source of truth for section presence, ordering, and content
quality.

## Sections (in order)

Every intent document carries these sections in this order. A
section is omitted only when it is genuinely empty (e.g., no open
questions remain after clarification).

### 1. Problem / Goal statement

One to three paragraphs naming the problem to solve or the goal to
reach. Lead with the user-facing outcome, not the implementation.
Answer: who is affected, what is broken or missing, why now.

### 2. Confirmed decisions (numbered, with rationale)

Each binding decision is a numbered item with two parts:

```
1. <decision statement>

   Rationale: <why this decision, citing discovery or clarification>
```

Binding decisions are those that constrain the implementation:
technology choice, API shape, scope boundary, compatibility
guarantee. Informational decisions (preferences, defaults) live in
the Constraints section instead.

### 3. Scope

Two explicit lists:

- **In scope**: what this intent covers. Atomic, verifiable items.
- **Out of scope**: what this intent explicitly does NOT cover, even
  if adjacent. Out-of-scope items prevent scope creep more
  effectively than vague in-scope items.

### 4. Constraints

Technical, temporal, and organizational limits the implementation
must respect. Group by category when the list is long:

- **Technical**: language version, platform, library availability,
  performance budget
- **Temporal**: deadlines, sequencing dependencies, freeze windows
- **Organizational**: team ownership, review requirements, compliance

### 5. Open questions (if any remain)

Specific, answerable questions that survived the clarification
phase. Each question identifies its blocker (what cannot proceed
without the answer) and who likely owns the answer.

### 6. Acceptance criteria (numbered, verifiable)

Each criterion is a single, atomic, testable statement. Numbered for
traceability into work items and tasks.

```
1. <verifiable statement>

   Verification: <how to check — test name, command, observation>
```

## Acceptance Criteria Guidance

**Atomic**: one criterion, one behavior. If a criterion contains
"and", split it.

**Testable**: each criterion has a concrete verification path — a
test, a command output, an observable behavior. "Works correctly"
fails this test; "returns 200 within 50ms at p99" passes.

**Verifiable not aspirational**: criteria describe the end state, not
the effort. "Reduce latency by 30%" is verifiable; "optimize
performance" is not.

**Numbered**: stable numbering supports traceability. Work items
reference criteria by number (`accepts: intent#3`).

## Scoping Guidance

**In scope belongs** when:

- The item is necessary to satisfy the goal
- The item's absence would make the intent incomplete
- A downstream implementer cannot proceed without knowing the item's status

**Out of scope belongs** when:

- The item is plausible but not required (avoids implicit scope creep)
- The item is explicitly deferred to a later intent
- The item was considered and rejected (records the decision)

**Vague scope items fail validation**. "Improve UX" is vague;
"Add keyboard shortcut for the top 5 commands" is scoped.

## Decision Documentation

**Binding vs informational**:

- **Binding**: constrains the implementation. Lives in "Confirmed
  decisions" with rationale. Example: "Use SQLite, not Postgres."
- **Informational**: provides context but does not constrain. Lives
  in "Constraints". Example: "Team prefers early returns."

**Rationale requirements**:

- Cite the source (discovery report, clarification answer, prior work
  item) when the decision was informed by research
- State what alternatives were rejected and why, when alternatives
  were considered
- A bare "we chose X" without rationale is a validation failure

## Open Question Phrasing

**Specific, not vague**:

- Good: "Should the new endpoint return 200 with a degraded payload
  when the upstream is unavailable, or 503 with retry guidance?"
- Bad: "What should we do about errors?"

**Identify the blocker**:

- Each open question names what work it blocks and which downstream
  phase cannot proceed without the answer.

**Identify the owner**:

- Each open question names the role or person likely to own the
  answer, so the gap is actionable.

**Minimal**: open questions should be rare after clarification. If
more than 3-4 open questions remain, the intent likely needs another
clarification round, not publication.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Missing rationale on decisions | Untestable later | Cite discovery or clarification source |
| Vague acceptance criteria | Cannot verify completion | Atomic + testable statement |
| No out-of-scope section | Implicit scope creep | Explicit "Out of scope" list |
| Many open questions | Premature publication | Run another clarification round |
| Mixing binding and informational decisions | Confusing authority | Binding in decisions, informational in constraints |
| Implementation detail in goal statement | Premature constraint | Keep goal at outcome level |

## Verification-Point Decisions

Decisions about where the work will be tested or verified are binding
decisions: state them in "Confirmed decisions" with rationale, like
any other constraint on the implementation. Name the points where
the change's behavior will be observable — an existing command, an
API surface, a rendered artifact — and prefer existing points over
new ones, keeping the count minimal: the ideal is one point where the
whole behavior can be checked end to end. Tests verify the externally
observable behavior at those points, not the implementation behind
them. Confirm the chosen points with the user during clarification,
since they shape how the acceptance criteria will be proven.

## No Stale Paths or Code Snippets

Intent documents do not carry file paths or code snippets. Both go
stale quickly — a path survives one refactor, a snippet survives
none — and a stale example misleads the implementer it was meant to
help. State the decision the path or snippet was illustrating; the
implementer locates its current home in the tree.

**Exception (prototype snippets)**: when a prototype produced a
snippet that encodes a decision more precisely than prose can — a
state machine, a schema, a type shape — inline it within the
relevant decision and note briefly that it came from a prototype.
Trim to the decision-rich parts, not a working demo.
