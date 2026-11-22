---
description: "Teaching workspace — run a stateful, multi-session teaching engagement for one topic, keeping its mission, resources, glossary, lessons, and learning records in a workspace the user returns to."
binds_to_depth: main
---

# Teach

Run a stateful, multi-session teaching engagement: build the user a
teaching workspace that outlives any single session, then teach the
next right thing each time they return. This is a main-agent-only
orchestrator skill — the main agent interviews, decides, and
dispatches; sub-agents do all other work.

## Orchestration Context

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Delegation Mandate

The main agent performs no research, no bash, and no extraction.
Every work class below is dispatched to a sub-agent; the main agent
frames the task, passes paths and user answers, and relays results.

| Work class | Dispatched to |
|---|---|
| Finding, vetting, and summarizing learning resources | research/discovery sub-agents |
| Inspecting workspace state (bootstrap, progress calculation) | discovery sub-agents |
| Authoring lessons, reference documents, and workspace files | implementation/procedure sub-agents |
| Creating directories, opening lesson files, all file operations | implementation/procedure sub-agents |
| Mission interview and clarification rounds with the user | the main agent — never delegated |

Work classes are sequential by default: a research sub-agent's report
path feeds the authoring sub-agent that builds the lesson from it.
The main agent passes report paths between launches, never content.

## Session Protocol

1. **Resolve the workspace** (next section), then bootstrap: launch a
   discovery sub-agent to inspect the workspace and report which
   state files exist and what each contains (mission present?
   resources populated? glossary started? how many learning records
   and lessons?).
2. **Ground the mission.** If the mission file is missing or vague,
   interview the user until the reason they are learning this topic
   is concrete and observable. Push back on vagueness once: a bad
   mission steers every later lesson wrong.
3. **Teach the next thing.** Loop:
   - Launch a discovery sub-agent to read the learning records and
     report where the user's edge currently sits.
   - Pick the single lesson that fits that edge and serves the
     mission.
   - Launch a research sub-agent to gather and vet source material
     for it.
   - Launch an implementation sub-agent to author the lesson, any
     reference document it compresses to, and the shared assets it
     can build from.
   - After the user demonstrates the lesson's win, launch an
     implementation sub-agent to record the learning record.
4. **Close the session** by relaying what changed in the workspace
   and what the likely next lesson is.

## Philosophy

Deep learning needs three things, and the workspace serves all
three:

- **Knowledge**, captured from high-quality, high-trust resources
- **Skills**, acquired through interactive lessons built on that
  knowledge
- **Wisdom**, which comes from real-world communities of practice

Some topics lean knowledge-shaped (theoretical physics); others lean
skill-shaped (yoga). Weight lessons accordingly.

`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`

Split two kinds of learning and design for the second:

- **Fluency strength**: in-the-moment retrieval — feels like mastery,
  often is not
- **Storage strength**: long-term retention — the actual goal

Build storage strength with desirable difficulty: retrieval practice
(recall from memory), spacing (practice distributed over time), and
interleaving (mixing related topics — skills practice only).

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

Calibrate each lesson to the user's zone of proximal development:
challenging just enough. When the user has not named the next thing,
a discovery sub-agent reads the learning records and reports the
edge; the main agent picks the most mission-relevant lesson that fits
it.

## Lessons and Assets

A lesson is one self-contained HTML file in the workspace, teaching
one tightly-scoped thing tied to the mission. The quality bar:
short enough to finish in one sitting, built toward a single tangible
win, beautiful enough to revisit, and linked to its sources. The
full bar, the knowledge-vs-skills acquisition posture, and the
authoring recipes live in
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::teach/reference/lesson_authoring.md" />.

Lessons are assembled from reusable components — a shared stylesheet
first, then quiz widgets, simulators, diagram helpers. Reuse is the
default: the authoring sub-agent builds from the asset inventory
before writing anything new.

## Workspace Contract

The workspace root arrives as an invocation argument. When the
current working directory is a git repository, ask the user once
where the workspace should live before creating anything — never
scatter teaching state into a repository. The workspace is plain
files the user can read and keep: a mission, curated resources, a
glossary, lessons, learning records, shared assets, and a notes
scratchpad. The complete state map and every file format are
specified in
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::teach/reference/workspace_layout.md" />.

Missions change as the user grows. Confirm the shift with the user,
then dispatch an implementation sub-agent to update the mission file
and add the learning record that captures the change.

## Anti-Patterns

| Avoid | Why | Instead |
|---|---|---|
| Trusting your parametric knowledge as lesson content | It is unverifiable and often stale | Research sub-agents ground every lesson in vetted sources |
| Counting coverage as learning | Exposure is not retention | Record a learning record only on demonstrated understanding |
| Skipping the mission interview | Ungrounded lessons drift abstract | Interview until the mission is concrete before teaching |
| The main agent researching or writing lessons itself | Breaks the delegation mandate and bloats orchestrator context | Dispatch research and authoring sub-agents with report paths |
| One lesson teaching many things | Working memory overflows; nothing sticks | One lesson, one win, tied to the mission |
| Inlining what a second lesson could reuse | Duplicated components drift apart | Author new reusable pieces as workspace assets |
