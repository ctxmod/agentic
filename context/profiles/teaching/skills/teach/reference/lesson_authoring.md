# Lesson Authoring

How lessons get researched, authored, and assembled in a teaching
workspace. The main agent dispatches; the sub-agent recipes below
are the launch contracts for each step.

## The Lesson Quality Bar

A lesson is one self-contained HTML file in `lessons/`, numbered
`0001-<dash-case-name>.html` with the number incrementing each time.
The authoring sub-agent builds every lesson to this bar:

- **One win.** The lesson teaches one tightly-scoped thing and gives
  the user a single tangible win they can build on. Learners'
  working memory is small — stay inside it.
- **Mission-tied.** Every lesson traces to the mission; if it does
  not serve the mission, it is out of scope.
- **Edge-fitting.** The lesson sits in the user's zone of proximal
  development: challenging just enough, built on what learning
  records show is already known.
- **Beautiful and revisitable.** Clean, readable typography and
  layout — the user returns to lessons later. Think Tufte.
- **Cited.** Lessons carry citations: links to external sources
  backing every claim, plus one recommended primary source (the
  highest-trust resource found on the topic).
- **Connected.** Each lesson links to related lessons and reference
  documents, and reminds the user they can ask the agent followup
  questions — the agent is their teacher.

The authoring sub-agent finishes each lesson by writing any
compressed reference document it yields (see Reference Documents
below) so the durable essence outlives the lesson.

## Knowledge Versus Skills

Design around the skill the user is acquiring; include only the
knowledge that skill requires. Teach the knowledge first, then let
the user practice through an interactive feedback loop.

- For **knowledge acquisition**, difficulty is the enemy — it eats
  the working memory needed for understanding. Sources must be
  clear and trusted.
- For **skill acquisition**, difficulty is the tool — effortful
  retrieval builds long-term retention. Interactive lessons with
  quizzes and light in-browser tasks, or guided real-world steps,
  all wrapped in the tightest possible feedback loop: immediate,
  ideally automatic.

Quiz-craft rule: every answer choice should be the same length, so
formatting never leaks the answer.

## Reference Documents

Lessons are rarely revisited; reference documents are. While
authoring a lesson, the sub-agent also compresses its durable
content into a print-ready quick-reference under `reference/` —
syntax and snippets for programming, algorithms and flowcharts for
processes, poses and sequences for physical practice, exercises and
routines for fitness, glossaries for any topic with its own
nomenclature. A glossary, once created, is adhered to in every
later lesson.

## Assets and Component Reuse

Lessons are assembled from reusable components in `assets/`:
stylesheets, quiz widgets, simulators, diagram helpers — anything a
second lesson could reuse.

- The shared stylesheet is the first component every workspace
  earns: every lesson links it, so the course looks like one course.
- Before authoring, the sub-agent reads the asset inventory and
  builds from what exists.
- When a lesson needs something new and reusable, the sub-agent
  writes it as a component in `assets/` and links to it — never
  inlines code a future lesson would duplicate.

## Dispatch Recipes

**Research step** — launch a research sub-agent with: the lesson's
topic and its mission tie, the relevant `RESOURCES.md` entries and
`## Gaps` rows, and a report path. It vets sources, returns the
strongest primary source plus supporting material, and proposes
`RESOURCES.md` additions with annotations.

**Authoring step** — launch an implementation sub-agent with: the
research report path, the glossary and asset inventory, the target
lesson number, and the quality bar above. It writes the lesson, the
reference documents it yields, and any new reusable assets, and
reports what it created.

**Record step** — after the user demonstrates the lesson's win,
launch an implementation sub-agent to write the learning record
following the format in
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::teach/reference/workspace_layout.md" />.

## Wisdom Through Communities

When a question needs real-world testing, attempt an answer, then
route the user to a community — a forum, a well-moderated group, a
local class. Research sub-agents find high-reputation options and
record them under Wisdom in `RESOURCES.md`. If the user opts out of
communities, respect it and note the opt-out.
