# Phase Information Flow

Information moves between workflow phases through three mechanisms: parameter passing, output chaining, and inherited state. Each carries different costs.

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

Main agents launch sub-agents through the harness's agent-launch tool (in Claude Code, the Agent tool). Sub-agents have no memory of prior phases--all behavior comes from custom context provided in the prompt.

## The Brief Contract

A sub-agent cannot ask what its parent meant, so every brief states five things the template components below carry:

1. **Objective** — the question to answer or the change to make, and why it matters to the parent.
2. **Output format** — what the return holds (a summary plus a report path) and the report's structure.
3. **Tool and source guidance** — which tools and sources to use first, and which to avoid.
4. **Boundaries** — what is out of scope (Delimitations below), including files and decisions reserved for the parent.
5. **Effort bound** — a budget scaled to the task: a fact lookup is one agent and a handful of tool calls; a comparison is a few agents with a bounded call budget each; open-ended research earns more. Without a stated bound, agents both under-invest in hard tasks and over-invest in easy ones.

Briefs missing these produced duplicated work across siblings, coverage gaps, and agents that misread a short task description as a different task.

## Template Components

**model**: the small, fast tier for story-completion validation (the story-completion validator); the mid tier for information gathering and straightforward analysis; the strongest tier for complex reasoning, implementation requiring judgment, difficult debugging — genuinely complex interpretation, not routine validation; do not promote validation work to a higher tier by default. (In Claude's model family the tiers are haiku, sonnet, and opus.)

**prompt**: Contains all context the sub-agent needs.

**Compulsory reads**: Files that are relevant for the sub-agent. Use the harness's file-inlining syntax where it has one (Claude Code: `@<path>`; for rendered context, the path of the rendered file) so files are immediately inlined; otherwise list the paths as required first reads.

| File Type | Example |
|-----------|---------|
| Objectives | the `discovery_objective` skill |
| Schemas | `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md` |
| Prior reports | `{REPORTS_BASE}/01-discovery.md` |
| Project context | repo-local tool docs from the repository's own context module (not part of this module) |
| Intent files | `@{INTENT_PATH}` |
| Tool queries | `ctxmod work show <id>`, `ctxmod work list --kind report --owner <id>` |

**Parameters**: Variables for output paths and scope.

| Parameter | Purpose |
|-----------|---------|
| Report (reserved) | Sub-agent creates the report record just-in-time with `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path |
| INPUT_REPORT | Prior phase output to consume |
| SCOPE | What to include |
| ARTIFACT_PATHS | Targets for validation or modification |

**Delimitations**: Explicit boundaries--what is out of scope.

**Notes**: Special handling, known issues, caveats.

## Information Categories

| Category | Behavior | Examples |
|----------|----------|----------|
| Direct | Changes per phase | Report paths, scope definitions, input references |
| Inherited | Stable across workflow | Project conventions, quality thresholds, work item paths |
| Chained | Output becomes input | Reports, findings, modified artifacts |
| Intent | Stable across workflow | Developer intent, requirements, constraints, acceptance criteria |

## Parameter Flow

`ctx:github.com/ctxmod/agentic/context::shared::framework@workflows/reference/params.md`

**Direct assignment** -- phase-specific parameters change each launch:

```
Phase 1: Report: sub-agent reserves just-in-time via `ctxmod work new report "01-discovery" --owner <work item, if any>` immediately before writing; returns the path, SCOPE="..."
Phase 2: Report: sub-agent reserves just-in-time via `ctxmod work new report "02-analysis" --owner <work item, if any>` immediately before writing; returns the path, INPUT_REPORT={Phase 1 output}
```

**Inheritance** -- work context parameters (EPIC_PATH, STORY_PATH, ISSUE_PATH, project conventions, quality standards) remain constant across all sub-agent launches. Main agents include them automatically without sub-agents requesting them.

**Intent context** -- main agent creates the intent records during preflight (the developer's instruction as an `intent` source, the intent document as a `criteria` artifact, both owned by the most specific work item, if any — the same ownership rule report records follow) and passes the document's `file` as `INTENT_PATH`. Main agents include them automatically without sub-agents requesting them. Passed to all sub-agents needing developer intent. See the Intent Path section of `params` in the agentic `workflows` profile's `framework` package.

## Durable Decisions vs Transient Observations

Durable artifacts are rehydration state: git history, work items, intent
records, decisions, source, and reference documentation let a fresh
session resume work. Transient observations — raw command output, large
scans — are disposable once their decision-relevant result is recorded.
Record the result, not the transcript: a decision written into source,
the work store, or the context module's `reference/` survives; a command transcript does not
need to.

## Output Chaining

Phase outputs become subsequent phase inputs. Pass references rather than content--the orchestrator's budget stays lean and sub-agents read selectively.

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`

**Sequential**: `Phase 1 writes: {REPORTS_BASE}/01-discovery.md` -> `Phase 2 receives: INPUT_REPORT={REPORTS_BASE}/01-discovery.md`

**Parallel**: `Parallel outputs: {REPORTS_BASE}/fan-out/*.md` -> `Synthesis receives: INPUT_REPORTS={REPORTS_BASE}/fan-out/*.md`

| Selection Strategy | When |
|--------------------|------|
| Full reference | Complete prior output needed |
| Section reference | Specific findings sufficient |
| Summary | Awareness without detail |
| None | Prior output irrelevant |

**Artifact synthesis**: Final phases synthesize reports (intermediate findings) into artifact records — `ctxmod work new <kind> "<title>" --owner <work item>` with a kind such as `synthesis`, `plan`, `summary`, or `handoff`. Artifacts represent cumulative work and provide standalone value without requiring readers to review underlying reports. Artifacts link back to precursor reports. See the Artifacts section of `params` in the agentic `workflows` profile's `framework` package.

## Tool-Based Context

The ctxmod work store provides structured work item data via CLI. Sub-agents use `ctxmod work` commands instead of manual file I/O for:

- Work item discovery and inventory (`ctxmod work list`, filtered by `--kind`, `--status`, `--parent`, `--owner`)
- Context retrieval for specific items (`ctxmod work show <id>`)
- Report and intent path resolution (`ctxmod work list --kind report --owner <id>`, `ctxmod work list --kind intent --owner <id>`)
- File reference lookups (search record bodies under the store directory `ctxmod state show` reports, e.g. `rg -l <path> <store-dir>`)
- Status statistics (`ctxmod work status`)

This fits the reference-over-inline principle: `ctxmod work` returns paths and structured metadata (`--stdout-format json` for machine-readable output); sub-agents read specific files only when needed. Main agents may inline `ctxmod work` output directly in sub-agent prompts (cheap, structured) rather than passing full work item file content.

`work_store_first` in the ctxmod builtin `work_discipline` profile

## Accumulation Patterns

| Pattern | Receives | Trade-off | Use Case |
|---------|----------|-----------|----------|
| Bounded | Related predecessors only | Prevents growth; other outputs accessible via explicit reference | Default for most workflows |
| Accumulated | All prior outputs | Enables cross-phase reasoning at larger transfer cost | Cross-phase analysis required |
| Selective | Per-phase choice | Tailored context per phase | Verification receives discovery + implementation (compares findings against changes) |

## Pattern Integration

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/patterns_overview.md" />

| Pattern | Flow | Output |
|---------|------|--------|
| Sequential | Linear chain | Predecessor output |
| Parallel | Shared input, independent execution | Aggregation phase receives all |
| Validation Loop | Artifacts cycle back | Until criteria pass |
| Fan-Out Fan-In | Distributed exploration | Centralized synthesis |

## Efficiency

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`

A 500-line report inlined costs 500+ tokens per phase; a reference costs ~10. Three strategies minimize transfer cost:

**Reference over inline**: Pass file paths. Phases read selectively based on actual needs.

**Selective loading**: Load required content fully, context sections as needed, optional content on demand:

| Content Category | Loading | Trigger |
|------------------|---------|---------|
| Discovery report | Full | Required input |
| Project conventions | Sections | Context needed |
| Prior analysis | On demand | Optional reference |

**Boundary summaries**: Compress long outputs for orchestration awareness while preserving detail through reference. Example: Full `{REPORTS_BASE}/01-discovery.md` (1,500 words) becomes summary: "Found 12 patterns in 4 modules. Key gap: error handling."

Storing sub-agent output on disk and passing a reference is also a fidelity measure, not only a budget one: every relay of content through an intermediate agent's paraphrase loses detail (the game of telephone), while a path hands every downstream reader the authoritative text.

## Validation Points

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_before_execution.md" />

Story completion is the one mandatory high-level validation boundary, owned by the Haiku-class story-completion validator — not per-phase validator fan-out. Between phases, run risk-based local checks: cheap, deterministic, in your own tree. A local check that fails halts the workflow — invalid state compounds through subsequent phases.

- **Before launch**: Required reports exist, paths resolve, parameters valid
- **After completion**: Output written to expected path, format correct, no unexpected modifications
- **Across transitions**: Parameters consistent, references valid, no corruption
- **At story completion**: The story-completion validator runs the full closure in one session — scoped deterministic checks, in-scope repair, recheck, and the commit itself

## Mistakes

| Mistake | Consequence | Alternative |
|---------|-------------|-------------|
| Inline all prior outputs | Budget exhaustion | Reference and load selectively |
| Skip boundary validation | Malformed data propagates | Validate before proceeding |
| Unbounded accumulation | Exceeds limits | Bound with on-demand access |
| Discard prior outputs | Later phases lack needed data | Maintain references |
| Inconsistent parameter names | Lookup failures | Standard naming conventions |
| Main agent reads sub-agent reports | Context bloat | Pass report paths between sub-agents |

REPORTS_BASE (the owning work item of report records) derives from work item context per `ctx:github.com/ctxmod/agentic/context::shared::framework@workflows/reference/params.md`.

## Sources

- https://www.anthropic.com/engineering/multi-agent-research-system — what each delegated agent needs (objective, output format, tool and source guidance, task boundaries), effort scaled to query complexity, sub-agent output stored externally to avoid the game of telephone (Anthropic Engineering, 2025-06-13; accessed 2026-11-22)
- https://code.claude.com/docs/en/sub-agents — vendor example of an agent-launch tool and per-agent model selection (Claude Code docs; accessed 2026-11-22)
