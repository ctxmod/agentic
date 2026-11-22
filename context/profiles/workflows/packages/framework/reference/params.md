# Orchestration Parameters

Main agents pass standard parameters to sub-agents to ensure outputs land at correct locations within the active work context. Work state lives in the ctxmod work store (`ctxmod work …`); every parameter below names records in that store.

## Work Item Parameters

Work item parameters define the current scope. Main agents provide only parameters relevant to the active level. **Unused levels are always omitted**--this constraint prevents context pollution and ensures sub-agents receive minimal necessary context.

### Parameter Hierarchy

| Parameter | Kind | Scope | Owns |
|-----------|------|-------|------|
| EPIC_PATH | `epic` | Initiative | Reports, intents, artifacts; stories stand beneath it by `parent` |
| STORY_PATH | `story` | Feature | Reports, intents, artifacts; issues stand beneath it by `parent` |
| ISSUE_PATH | `issue` | Work unit | Reports, intents, artifacts; tasks stand beneath it by `parent` |
| TASK_PATH | `task` | Activity | Reports, intents, artifacts |

Each level owns the records created with `--owner` set to it; they live in its item directory (`reports/`, `intents/`, `plans/`, … beneath it).

### Parameter Format

Each work item parameter carries the item's record id and its item directory, as `ctxmod work show <id>` reports them (`file` is the item's `ITEM.md`; its directory is the item directory):

| Parameter | Format | Example |
|-----------|--------|---------|
| EPIC_PATH | `<epic id> (<item dir>)` | `vdwgsgzfrs (context.local/work/items/2026-02/22-0800-vdwgsgzfrs-platform-architecture/)` |
| STORY_PATH | `<story id> (<item dir>)` | `kpqrstbcdf (context.local/work/items/2026-02/22-0815-kpqrstbcdf-auth-overhaul/)` |
| ISSUE_PATH | `<issue id> (<item dir>)` | `mnbvcxzqwr (context.local/work/items/2026-02/22-0830-mnbvcxzqwr-token-refresh/)` |
| TASK_PATH | `<task id> (<item dir>)` | `zxcvbnmpqr (context.local/work/items/2026-02/22-0900-zxcvbnmpqr-input-validation/)` |

A record id is ten letters; any prefix of at least four letters that one record alone carries also names it, and `ctxmod work show` reports the shortest such prefix. Pass the full id in briefs.

**Containment rules**: Items never nest physically — every item lives at `items/<YYYY-MM>/<entry>/` and the hierarchy is the `parent` field (an epic has no parent; a story's parent is an epic; an issue's is a story or epic; a task can stand beneath any other item). `ctxmod work set <item> parent=<item>` re-parents without moving a file. Records created with `--owner` (reports, intents, artifacts) do nest physically, inside their owner's item directory.

### Item Statuses

Item state lives in the record's state file, never in body frontmatter: `ctxmod work set <id> status=<status>` changes it and `ctxmod work show <id>` reads it. The store's item vocabulary maps onto the lifecycle the workflows use:

| Lifecycle stage | Store status | Meaning |
|-----------------|--------------|---------|
| Draft (created, not started) | `open` | The first status of every new item |
| Active (in progress) | `in_progress` | Execution has started |
| Blocked | `blocked` | Waiting on a blocker the item names |
| Completed | `done` | Criteria met and verified |
| Cancelled | `cancelled` | Abandoned deliberately |

Artifacts (reports, plans, …) use `draft` and `final`. Any status of a vocabulary may follow any other: the store enforces the vocabulary, not a workflow — the lifecycle order is this module's doctrine.

### Context Scoping

| Working on... | Parameters provided |
|---------------|---------------------|
| General work | None |
| Epic E01 | EPIC_PATH |
| Story S03 within Epic E01 | EPIC_PATH, STORY_PATH |
| Issue I07 within Story S03 | EPIC_PATH, STORY_PATH, ISSUE_PATH |
| Task T02 within Issue I07 | EPIC_PATH, STORY_PATH, ISSUE_PATH, TASK_PATH |
| Standalone issue I12 | ISSUE_PATH |

(E01, S03, I07, T02, I12 are illustrative labels; the parameters carry record ids.)

## Output Ownership Derivation

All output records (reports, intents, artifacts) take their owner from the most specific work item parameter using a consistent fallback chain:

| Most specific context | Owner (`--owner`) | Lands in |
|-----------------------|-------------------|----------|
| TASK_PATH provided | the task | `<task item dir>/<category>/<YYYY-MM>/` |
| ISSUE_PATH provided (no task) | the issue | `<issue item dir>/<category>/<YYYY-MM>/` |
| STORY_PATH provided (no issue) | the story | `<story item dir>/<category>/<YYYY-MM>/` |
| EPIC_PATH provided (no story) | the epic | `<epic item dir>/<category>/<YYYY-MM>/` |
| No work item context | none | the store's top-level `<category>/<YYYY-MM>/` |

Where `<category>` is the record kind's directory: `reports` for a report, `intents` for an intent source, `plans`, `syntheses`, `summaries`, `handoffs`, … for artifacts (`ctxmod work kinds` lists them).

### Placeholders

| Placeholder | Filled by | Example | Notes |
|-------------|-----------|---------|-------|
| `{YYYY-MM-DD}` or `{date}` | Main agent at launch, when a brief needs it | `{date}` | Current date from the host (e.g. `date -u +%F`); entry names are stamped by `ctxmod work new` itself |
| `{HH-mm}` or `{time}` | Main agent at launch, when a brief needs it | `{time}` | Current time (24-hour) from the host; entry names are stamped by `ctxmod work new` itself |
| `{type}` | Varies | `discovery`, `validation`, `analysis` | Report classification; determined by sub-agent from objective type and carried in the report title (and an optional `--set type=<type>` field) |
| `{slug}` | Derived | `source`, `edge-cases`, `api-review` | Derived by the store from the record title at creation. Report titles: sub-agent (in its creation call). Intent titles: main agent (descriptive) |

**Mandatory pre-resolution**: Main agents MUST resolve the descriptive parts of anything the dispatch itself creates (the intent record's title) before launch. Report records carry nothing to resolve at launch — the sub-agent's just-in-time `ctxmod work new report` stamps date and time at creation. The report title (hence slug) is chosen by the sub-agent; intent titles are determined by the main agent. The slug is derived from the title, never appended by hand.

## Report Parameters

### REPORT_PATH (sub-agent reserved, never pre-assigned)

Dispatchers do not pre-assign report paths. Every sub-agent that produces reports reserves its own report path just-in-time — immediately before writing — by creating the report record, passing the owning work item as `--owner` when one exists. Reserving a report path means exactly this: `ctxmod work new report` mints the id, publishes the state file exclusively, and prints the `file` to write; the printed path is the reserved report path. The store is the SSOT for collision-free paths, and the `<DD-HHMM>-<id>-<slug>.md` entry name never trips a sub-agent report-file Write guard (which blocks only basenames starting with REPORT, SUMMARY, FINDINGS, or ANALYSIS). Paths still flow between phases via return messages: the sub-agent creates, writes, and returns its path; the parent passes that returned path onward and never mints one.

**Reservation** (immediately before writing):

```sh
ctxmod work new report "<title>" --owner <most specific work item id>   # omit --owner outside work-item context
```

**Resolved location**: `<owner item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-<slug>.md`, or the store's top-level `reports/<YYYY-MM>/` without an owner, following the ownership chain above.

**Finalizing**: when the report is complete, `ctxmod work set <id> status=final` (report records start `draft`).

**Transitional tolerance**: a legacy dispatch that still provides an explicit REPORT_PATH parameter is honored — write to it as given. If the Write is blocked by a harness report-file guard (REPORT.md-style basenames trip it even inside the store), create your own report record as above instead and note the substitution in your return message. New dispatch templates must not mint REPORT_PATH parameters; this tolerance is marked for later removal.

### REPORT_SLUGS (optional)

Comma-separated list of slugs when multiple reports are expected. This is title/slug guidance only — never a pre-minted path. The sub-agent creates one report record per slug (one `ctxmod work new report` call per slug, titled after it) and produces one report per reserved path.

```
REPORT_SLUGS="source-analysis,doc-coverage,dependency-map"
```

Produces one reserved path per slug: `<DD-HHMM>-<id>-source-analysis.md`, `<DD-HHMM>-<id>-doc-coverage.md`, `<DD-HHMM>-<id>-dependency-map.md`.

### Report Discovery

After sub-agents write reports, they return the report path. Main agents may use `ctxmod work list --kind report --owner <id>` to verify report availability and enumerate prior reports for subsequent phases without globbing the work item's `reports/` directory or reading frontmatter manually. The listing returns each record's id, status, title, and `file`.

`work_store_first` in the ctxmod builtin `work_discipline` profile

### Intent Path

**Parameter**: `INTENT_PATH`

**Creation**: `ctxmod work new intent "<descriptive title>" --owner <most specific work item id>` — prints the payload `file`; the main agent writes the intent there.

Ownership follows the chain above, substituting the `intents` category for `reports`. Intent titles are descriptive (e.g., `auth-edge-cases`) rather than type-based--the main agent always determines the title during preflight.

**Examples**:

```
# Epic-level intent
context.local/work/items/2026-02/22-0800-vdwgsgzfrs-platform-architecture/intents/2026-02/22-0800-hgfdsqwrtz-platform-architecture.md

# Task-level intent
context.local/work/items/2026-02/22-0900-zxcvbnmpqr-input-validation/intents/2026-02/22-1506-bcdfghjkmn-auth-edge-cases.md
```

**Purpose**: Persistent intent specification written by main agent during preflight, scoped to work item context. An intent is a `sources` record: its payload is written once and sealed (the first `ctxmod work touch` after writing records its digest and makes it read-only), so a corrected intent is a new intent record. Sub-agents read as compulsory context but never modify. Multiple work items can have different intents. Work items reference intent records from their body's related list (by id or `file`).

**Main agent**: Create the intent record owned by the most specific work item, write the payload during preflight, seal it (`ctxmod work touch <id>`), pass INTENT_PATH to sub-agents.

**Sub-agent**: Read as compulsory context, reference throughout execution, do not modify.

**Directional responsibility**: Main agents WRITE intent records during preflight; sub-agents READ intent records during execution. This unidirectional flow ensures intent remains stable throughout workflow execution.

### Artifacts

**Location**: `<owner item dir>/<artifact category>/<YYYY-MM>/` (or the store's top level without an owner)

Artifacts are final products synthesized from multiple reports and informed by intent. No ARTIFACTS_PATH parameter exists--main agents direct artifact creation in final workflow phases when needed, with `ctxmod work new <artifact kind> "<title>" --owner <work item>`.

**Organization by kind**:

| Purpose | Kind (or option) | Examples |
|---------|------------------|----------|
| Runbooks, automation, utilities | an artifact record created with `--ext sh` (or the script's extension) | `deploy-production.sh`, `migrate-users.sh` |
| Synthesis, evaluations, analyses | `synthesis` | `auth-providers-evaluation.md` |
| Plans | `plan` | `architecture-proposal.md` |
| Work-in-progress, drafts | any artifact kind left at status `draft` | `api-spec-v2.yaml`, `architecture-proposal.md` |
| Handoffs and summaries | `handoff` (to siblings or successors), `summary` (to a parent) | `session-handoff.md` |

**Architectural distinction**: Artifacts differ fundamentally from reports. Reports are intermediate workflow outputs created frequently by sub-agents. Artifacts are final products created rarely through cumulative synthesis. This distinction drives the absence of an ARTIFACTS_PATH parameter--sub-agents rarely create artifacts, so main agents direct artifact creation explicitly when needed.

**Artifacts vs. reports**:

| Dimension | Reports | Artifacts |
|-----------|---------|-----------|
| Frequency | Frequent (workflow output) | Rare (finalized products) |
| Stage | Intermediate findings | Cumulative synthesis |
| Quality | Draft-level | Polished, standalone value |

**Workflow relationship**:

```
intents -> [work] -> reports -> [synthesis] -> artifacts
```

**Progression relationship**: Intent informs work. Work produces reports. Reports synthesize into artifacts. Each stage builds on the previous, creating a cumulative value chain from initial specification to final deliverable.

**Conventions**: Clear titles indicating purpose, links to precursor reports, standalone value (readable without reviewing underlying reports). No strict schema beyond the record kind.

## Parameter Passing

Main agents pass parameters in order: work item context first, then intent, then output parameters.

**Issue-level example**:

```markdown
## Parameters

ISSUE_PATH="mnbvcxzqwr (context.local/work/items/2026-02/22-0830-mnbvcxzqwr-token-refresh/)"
INTENT_PATH="context.local/work/items/2026-02/22-0830-mnbvcxzqwr-token-refresh/intents/2026-02/22-0915-tqwrtzpsdf-api-redesign.md"
```

**Task-level example** (full hierarchy):

```markdown
## Parameters

STORY_PATH="kpqrstbcdf (context.local/work/items/2026-02/22-0815-kpqrstbcdf-auth-overhaul/)"
ISSUE_PATH="mnbvcxzqwr (context.local/work/items/2026-02/22-0830-mnbvcxzqwr-token-refresh/)"
TASK_PATH="zxcvbnmpqr (context.local/work/items/2026-02/22-0900-zxcvbnmpqr-input-validation/)"
INTENT_PATH="context.local/work/items/2026-02/22-0900-zxcvbnmpqr-input-validation/intents/2026-02/22-0915-wxzqrtpsdf-input-validation.md"
```

There is no REPORT_PATH parameter: the sub-agent reserves its report path just-in-time, using the most specific work item parameter as the `--owner`. All output records take their owner from the most specific work item parameter.

## Responsibilities

**Main agent** (before launch):
1. Determine active work context and include relevant work item parameters
2. Create anything the dispatch itself owns (the intent record) and resolve its descriptive title; pass `{date}`/`{time}` only when a brief needs them
3. Include REPORT_SLUGS if multiple reports expected
4. Never pre-assign report paths — no REPORT_PATH parameter in briefs

**Sub-agent** (on receipt):
1. Extract work-item parameters from prompt context
2. Immediately before writing each report, reserve its path: `ctxmod work new report "<title>" --owner <most specific work item id>`; omit `--owner` outside work-item context
3. Write the report to the reserved path exactly as printed
4. Return the reserved report path (and record id) in the summary response

## Directory Structure

The store keeps every record's state in a state file and every body in a file its author writes. Under the default placement (`context.local/work/` at the repository's main worktree; `ctxmod state show` names the actual directory):

```
context.local/work/
├── meta/<xx>/<id>.yaml                      # state files (written only by ctxmod)
├── items/<YYYY-MM>/<DD-HHMM>-<id>-<slug>/   # one directory per item (epic, story, issue, task)
│   ├── ITEM.md                              # the item body
│   ├── reports/<YYYY-MM>/                   # owned reports
│   │   └── <DD-HHMM>-<id>-<slug>.md
│   ├── intents/<YYYY-MM>/                   # owned intent sources
│   ├── plans/, syntheses/, summaries/, handoffs/, …   # owned artifacts
├── reports/<YYYY-MM>/                       # reports with no owner
├── intents/<YYYY-MM>/                       # intents with no owner
├── plans/, syntheses/, …                    # artifacts with no owner
├── episodes/, facts/, procedures/, navigation/   # memories (never owned)
└── archive/                                 # archived bodies (`ctxmod work archive`)
```

**Hierarchy example** (parentage is a field, not nesting):

```
items/2026-02/22-0800-vdwgsgzfrs-platform-architecture/   # epic
items/2026-02/22-0815-kpqrstbcdf-auth-overhaul/           # story, parent=vdwgsgzfrs
items/2026-02/22-0830-mnbvcxzqwr-token-refresh/           # issue, parent=kpqrstbcdf
items/2026-02/22-0900-zxcvbnmpqr-input-validation/        # task, parent=mnbvcxzqwr
items/2026-02/23-1010-ptkrsdfghj-standalone-fix/          # standalone issue (no parent)
reports/2026-02/                                          # session-level (no work item)
```

Item bodies (`ITEM.md`) live at the root of their directories. Reports, intents, and artifacts nest within their owning work item's directory.

## Work Item Output Parameters

Writing objectives that produce work items create the item record and write its body:

| Objective | Creation | Writes |
|-----------|----------|--------|
| Epic writing | `ctxmod work new epic "<title>"` | the printed `ITEM.md` |
| Story writing (standalone) | `ctxmod work new story "<title>"` | the printed `ITEM.md` |
| Story writing (epic-nested) | `ctxmod work new story "<title>" --parent <epic id>` | the printed `ITEM.md` |
| Issue writing (story-nested) | `ctxmod work new issue "<title>" --parent <story id>` | the printed `ITEM.md` |
| Issue writing (standalone) | `ctxmod work new issue "<title>"` | the printed `ITEM.md` |
| Task writing | `ctxmod work new task "<title>" --parent <issue id>` | the printed `ITEM.md` |

When the main agent has already created the record (so its id can be passed to several agents), the writing objective receives the item parameter and writes `ITEM.md` in that item directory:

```
EPIC_PATH="vdwgsgzfrs (…/22-0800-vdwgsgzfrs-platform-architecture/)"  -> Writes: …/22-0800-vdwgsgzfrs-platform-architecture/ITEM.md
TASK_PATH="zxcvbnmpqr (…/22-0900-zxcvbnmpqr-input-validation/)"        -> Writes: …/22-0900-zxcvbnmpqr-input-validation/ITEM.md
```

## Omitting Parameters

**Missing REPORT_PATH** is the norm, not an orchestration error: sub-agents reserve their own report path just-in-time; outside work-item context the record lands in the store's top-level collection (`reports/<YYYY-MM>/`). A dispatch that still carries an explicit REPORT_PATH is transitional tolerance — honor it as given (see Report Parameters).

**Missing work item context** (appropriate for exploration): Sub-agents operate in general context--reports are created without `--owner` and land in the store's top-level `reports/`, findings stand alone without work item association. Appropriate for exploratory work or unscoped research.
