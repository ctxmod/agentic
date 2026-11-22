# Research Briefs

## Purpose

This page is for the main agent writing briefs for delegated web
verification — research and validation work handed to sub-agents. It
owns the conventions a brief carries — tool, route, and bound named
up front, plus the escalation ladder walked when a fetch fails — so
that delegated web verification stays bounded and its evidence stays
trustworthy without the orchestrator improvising remedies mid-flight.

## What every web-verification brief carries

Every web-verification brief names three things:

- the **tool** — the harness's web-reading tool is the default
  surface; a direct fetch CLI is the escalation surface;
- the **route** — the direct URL first, then the archive form of the
  same URL as the alternate;
- the **bound** — fetch scope and rate, stated so it covers the
  delegated agent's children too: "this bound applies to your
  children."

The motivating observation: in the session under analysis, 28 briefs
named no tool and no route, so every delegated agent invented both.

## The bounded escalation ladder

When a fetch fails, the ladder is walked in this exact order:

1. **retry-after-window** — if the verdict is a backoff or rate
   limit, wait out the window once and retry the identical URL (the
   recorded failure: "retry after the window expires" advice that
   was never actionable);
2. **archive URL form** — fetch the `web.archive.org` form of the
   identical URL (the recorded failure: an archive route improvised
   in one subtree reached one leaf and never its sibling, which kept
   grinding the same hosts);
3. **fetch CLI / refresh** — run the fetch CLI directly with its
   force-refresh (cache-bypass) option when a stale blocked-verdict
   cache is the suspected cause (the recorded fact: this remedy was
   CLI-only, unreachable from the web-reading tool);
4. **report limitation to parent** — after the ladder, report the
   limitation upward and stop; grinding, param-shuffle retries, and
   cache-buster retries cannot beat an egress verdict (recorded:
   retries on dead routes burned error text for zero content).

Terminal guard: never verify a claim against content the tool marked
degraded or untrusted.

These ladder behaviors are verifiable against a measurement
harness whose run records are provenance-digested and attributable to
a build. The harness and its corpus — an uncommitted, host-local work
artifact — are referenced as substrate, never edited or owned; the
citation rides the repository's test-tool contract.

## Evidence duties

1. **Absence claims require non-degraded sources.** "The page contains
   no X" judged over content the tool marked degraded asserts an
   absence the evidence cannot carry — the terminal guard binds every
   claim, not just ladder retries. Degradedness signals and the runnable
   remedies are owned by the fetch tool's own documentation.
2. **User-pasted content is recorded as provenance in final reports.**
   When the operator substitutes pasted text for a failed fetch — the
   session record ends with 203,092 bytes of judgment text across six
   interventions — the final report names which claims that text
   backs. The positive model in that record is the pointer pattern:
   persist the body, keep a short pointer inline, re-read from disk
   when a claim needs it.
3. **Persisted bodies are read before paragraph-level claims.** In
   that record, roughly 177 KB of fetched bodies sat unread on disk
   while a report asserted paragraph-level holdings over them, with
   "cross-checked against the supplied full text" language no actual
   cross-check backed. Persisting is not reading: read the body, or
   drop to pointer-level claims.

## Cascading bounds and the orchestrator self-check

1. **Bounds cascade to children.** A bound written into a delegated
   agent's brief is stated to cover the agent's retries and its
   children's retries — the template's phrase "this bound applies to
   your children" belongs in every bound. In the record, the 16:10
   "no broad fetching" bound stopped at the agent it addressed; the
   leaf beneath it ground nine more fetch failures after the bound.
   Dispatch language is owned by
   <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/subagent_mantra.md" />; this extends it,
   never restates it.
2. **The orchestrator self-check: verify, then ask once.** Before
   asking the operator about a filesystem fact, verify it with your
   own Read; then ask once, stating what you verified. In the
   record, staged DSAR PDFs were absent at the check, then delivered
   minutes later; the recorded mitigation is exactly this
   convention. Three neighbors it must not duplicate:
   `stale_inputs_and_drift` in the ctxmod builtin `delegation` profile (briefs
   are snapshots), <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />
   (dispatch and verification duties), and
   <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/report_passing.md" /> (report paths, work-item
   sequencing).

## Evidence provenance

The observations above anchor in a consolidated fetch-session
findings report, an uncommitted work-store analysis artifact, cited
descriptively, never linked. Its failure-mode
taxonomy table carries the root-cause rows: **RC11**, missing
fallback ladder, non-cascading bounds, no degraded-content guard;
and **RC13**, paragraph-level claims from unread persisted bodies.
The report's reconciled intervention timeline (§1.3) anchors the
self-check/ask-once mitigation and the paste-substitution stream.
