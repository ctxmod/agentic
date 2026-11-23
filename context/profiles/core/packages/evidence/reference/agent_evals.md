# Agent Evals

Deliberate verification (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />)
asks whether one piece of work is correct. An **eval** asks whether an
agent setup — model, harness, tools, and the context it loads (rules,
skills, briefs, tool descriptions) — does the job reliably across many
runs. Context changes are behavior changes: whether a sentence earns
its load is settled by running the agent with and without it, and an
eval is how that running becomes evidence instead of anecdote.

## Vocabulary

| Term | Meaning |
|---|---|
| Task | One test case: defined inputs and success criteria |
| Trial | One attempt at a task; run several, because outputs vary |
| Grader | Logic that scores one aspect of a trial; a task may have several |
| Transcript | The full record of a trial — outputs, tool calls, reasoning |
| Outcome | The environment's final state after the trial (the file exists, the record changed) |
| Suite | A set of tasks measuring one capability or behavior |

Evaluating an agent always evaluates the model and its harness
together; a result does not transfer to a different harness or context
set.

## Graders

Prefer deterministic graders; add model graders where judgement is
unavoidable; use human review to calibrate both.

| Grader | Strengths | Weaknesses |
|---|---|---|
| Code (string match, test pass, static analysis, outcome and tool-call checks) | Fast, cheap, objective, reproducible | Brittle to valid variation; little nuance |
| Model (rubric, natural-language assertions, pairwise comparison) | Flexible, handles open-ended output | Non-deterministic, costlier, needs calibration against human judgement |
| Human (expert review, spot checks) | Gold standard; calibrates the others | Slow, expensive, hard to scale |

Grading rules that keep the number honest:

- **Grade the outcome, not the path.** Check the end state and the
  artifact; a grader that demands one exact tool sequence penalizes
  valid alternatives.
- **Give partial credit** on multi-part tasks, so progress is visible.
- **Let a model grader say "unknown"** rather than forcing a verdict it
  cannot support.
- **Close shortcuts**: a task should be impossible to pass without
  solving it.
- **Isolate trials** in a clean environment each; shared state between
  trials leaks results across them.
- **Do not let the agent grade itself.** Self-evaluation is lenient,
  most of all on subjective quality; a separate grader, tuned toward
  skepticism, is the lever.

## Consistency metrics

- **pass@k** — the chance at least one of k trials succeeds; rises with
  k. Use it where one success is enough (a candidate a human then
  reviews).
- **pass^k** — the chance all k trials succeed; falls with k (75% per
  trial over three trials is about 42%). Use it where every run must
  work — an always-loaded rule, an unattended pipeline.

## Suites over time

- **Capability suites** start with a low pass rate and give a target;
  **regression suites** sit near 100% and catch drift. A capability task
  that passes reliably graduates into the regression suite.
- **Saturation**: a suite the agent always passes measures nothing new;
  add harder tasks rather than reading a flat 100% as progress.
- Give each suite an owner and maintain it like tests.

## Starting

1. Start early and small: twenty to fifty tasks drawn from **real
   failures** — the bug tracker, correction notes, lessons captured
   (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/lesson_capture.md" />).
   Early changes have large effects, so small sets detect them.
2. Write unambiguous tasks with a reference solution that proves each
   is solvable.
3. Balance the set: cases where the behavior should occur and cases
   where it should not (a skill that should trigger, and one that
   should stay quiet).
4. Measure a baseline without the change before measuring with it.
5. **Read the transcripts.** A failing score is either the agent's
   mistake or the grader's; only the transcript tells which.

## Sources

- https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents — task/trial/grader/transcript/outcome vocabulary, grader types, outcome-over-path grading, pass@k versus pass^k, capability versus regression suites, saturation, 20-50 tasks from real failures, "read the transcripts" (Anthropic Engineering, 2026-01-09; accessed 2026-11-22)
- https://www.anthropic.com/engineering/harness-design-long-running-apps — agents praising their own mediocre work; a separately tuned evaluator as the lever (Anthropic Engineering, 2026-03-24; accessed 2026-11-22)
- https://www.anthropic.com/engineering/multi-agent-research-system — small eval sets (~20 queries) suffice early; LLM-judge rubric; end-state evaluation for agents that change persistent state (Anthropic Engineering, 2025-06-13; accessed 2026-11-22)
- https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices — evaluation-first skill development against a no-skill baseline (Anthropic docs; accessed 2026-11-22)
