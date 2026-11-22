# Phase Information Flow

Information moves between workflow phases through three mechanisms: parameter passing, output chaining, and inherited state. Each carries different costs.

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

Main agents launch sub-agents using the Task tool. Sub-agents have no memory of prior phases--all behavior comes from custom context provided in the prompt.

### Template Components

**model**: haiku for story-completion validation (the story-completion validator). sonnet for information gathering and straightforward analysis. opus for complex reasoning, implementation requiring judgment, difficult debugging — genuinely complex interpretation, not routine validation; do not promote validation work to a higher tier by default.

**prompt**: Contains all context the sub-agent needs.

**Compulsory reads**: Files that are relevant for the sub-agent. Use the `@<path>` format (for rendered context, the path of the rendered file) so files are immediately inlined.

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
Phase 1: Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path, SCOPE="..."
Phase 2: Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path, INPUT_REPORT={Phase 1 output}
```

**Inheritance** -- work context parameters (EPIC_PATH, STORY_PATH, ISSUE_PATH, project conventions, quality standards) remain constant across all sub-agent launches. Main agents include them automatically without sub-agents requesting them.

**Intent context** -- main agent creates the intent source during preflight with `ctxmod work new intent "<title>" --owner <most specific work item, if any>` and passes its `file` as `INTENT_PATH` (the same ownership rule report records follow). Main agents include them automatically without sub-agents requesting them. Passed to all sub-agents needing developer intent. See the Intent Path section of `params` in the agentic `workflows` profile's `framework` package.

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
