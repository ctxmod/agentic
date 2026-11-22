# Teaching Workspace Layout

The workspace is a directory of plain files the user can read, keep,
and carry between sessions. An implementation/procedure sub-agent
creates and edits every file here; the main agent relays user
answers and review findings. Nothing outside this directory belongs
to the engagement.

## State Map

| Path | Role |
|---|---|
| `MISSION.md` | The reason the user is learning this topic; grounds every teaching decision |
| `RESOURCES.md` | The curated set of trusted sources and communities |
| `GLOSSARY.md` | The workspace's canonical language; every lesson adheres to it |
| `lessons/` | One self-contained HTML lesson per file, numbered `0001-<dash-case-name>.html` |
| `reference/` | Compressed, print-ready reference documents extracted from lessons |
| `learning-records/` | Numbered records of demonstrated understanding, `0001-<dash-case-name>.md` |
| `assets/` | Reusable lesson components (stylesheets, quiz widgets, simulators) |
| `NOTES.md` | The main agent's scratchpad for teaching preferences and working notes |

Create directories lazily — only when their first file is written.

## MISSION.md

The mission captures why the user is learning this topic. Every
decision — what to teach next, which resources to surface, which
exercises to design — traces back to it. The main agent interviews
the user until this is concrete, then an implementation sub-agent
writes the file.

```md
# Mission: {Topic}

## Why
{1-3 sentences: the concrete real-world goal. What changes in the
user's life or work when they have this skill? Avoid abstract
framings like "to understand X"; push for the underlying outcome.}

## Success looks like
- {A specific, observable thing the user will be able to do}
- {Another specific thing}

## Constraints
- {Time, budget, prior commitments, learning preferences — anything
  that bounds the approach}

## Out of scope
- {Adjacent topics the user explicitly does not want to chase now,
  protecting the zone of proximal development}
```

Rules:

- One mission per workspace. Two unrelated learning goals are two
  workspaces.
- Concrete over abstract: "Run a half marathon by October" beats
  "get fitter".
- Push back on vagueness once. If the user cannot articulate why,
  keep interviewing before writing anything; a bad mission is worse
  than no mission.
- Revise when reality shifts, confirming the change with the user
  first, and add a learning record capturing the shift.
- Keep it short: past one screen it has stopped being a compass and
  started being a plan.

## RESOURCES.md

The curated set of trusted sources. Lesson knowledge is drawn from
here, never from the agent's parametric guesses; wisdom comes from
the communities listed here. Research sub-agents populate and extend
it; they annotate every entry they add.

```md
# {Topic} Resources

## Knowledge

- [Book: _Title_ by Author](https://example.com)
  What it covers, and when to reach for it.
- [Article: "Title" by Author (Publisher)](https://example.com)
  What it covers, and when to reach for it.

## Wisdom (Communities)

- [Community name](https://example.com)
  Signal quality, and what to bring there (critique, troubleshooting).
- Local: {class or group}
  What real-time feedback it provides.

## Gaps
- {Areas the mission needs that no good resource covers yet — this
  drives the next research dispatch}
```

Rules:

- High-trust only: primary sources, recognised experts, peer-reviewed
  work, well-moderated communities. Marketing dressed as education
  stays out.
- Annotate every entry — a bare link is useless in three months.
- Group by Knowledge and Wisdom; a resource may appear in only one.
- Surface gaps explicitly under `## Gaps` so future research
  dispatches have targets.
- Prune ruthlessly: a source that proved wrong, shallow, or
  off-mission is removed, not buried.
- Record community opt-outs here so later sessions stop proposing
  them.

## GLOSSARY.md

The workspace's canonical language. All lessons, exercises, and
learning records adhere to its terminology. Building it is itself
part of learning: compressing a concept into a tight definition is
evidence the user understands it.

```md
# {Topic} Glossary

{One or two sentences on what this glossary covers.}

## Terms

**Term**:
{One or two sentences: what the term IS.}
_Avoid_: {alias}, {alias}
```

Rules:

- Add a term only once the user understands it — the glossary
  records compressed knowledge, it does not teach.
- Be opinionated: pick the best word for a concept and list the rest
  as aliases to avoid.
- Keep definitions tight, and use the glossary's own terms inside
  other definitions.
- Group under subheadings when natural clusters emerge.
- Flag ambiguities explicitly: if the wider field uses a term
  loosely, note the workspace's resolution.
- Revise as understanding deepens; no stale entries.

## Learning Records

Learning records live in `learning-records/`, numbered sequentially
`0001-<dash-case-name>.md`, `0002-...`. They are the teaching
analogue of architectural decision records — software repositories keep
the same discipline for their own decisions; see
`ctx:github.com/ctxmod/engineering/context::shared::structure@repositories/reference/adr_discipline.md`.
Records capture non-obvious lessons, key insights, and stated prior
knowledge that steer future sessions; an implementation sub-agent
writes each one after the user demonstrates understanding.

```md
# {Short title of what was learned or established}

{1-3 sentences: what was learned, and why it changes what to teach
next.}
```

That is the whole format — one paragraph is a complete record. The
value is capturing that something is now known and why it changes
the next lesson, not filling out sections.

Optional sections, only when they add real value:

- **Status** frontmatter (`active` or `superseded by LR-NNNN`):
  when earlier understanding turns out wrong and is replaced.
- **Evidence**: how the user demonstrated the understanding — a
  question answered, an exercise completed, prior experience cited.
- **Implications**: what this unlocks or rules out for future
  sessions.

Write a record when the user demonstrated genuine understanding of
something non-trivial, disclosed prior knowledge (with its claimed
depth), had a misconception corrected, or shifted the mission. Do
not write one for material merely covered — coverage is not
learning, wait for evidence; for anything already captured tersely
in GLOSSARY.md; or as a session-by-session activity log.

When a later record contradicts an earlier one, mark the old record
`superseded by LR-NNNN` rather than deleting it: the history of how
understanding evolved is itself signal.
