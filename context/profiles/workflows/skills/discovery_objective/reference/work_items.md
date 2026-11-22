# Work-Item Discovery Objective

Discover and catalog work items across epics, stories, issues, and tasks. This variant analyzes hierarchy integrity, status distribution, dependencies, and structural patterns to assess work tracking health.

## Base Behavior

the `discovery_objective` skill

## Work-Item Categories

### Inventory Classification

Catalog work items by kind (epic, story, issue, task), identifier format (ctxmod record ids are ten letters of `bcdfghjkmnpqrstvwxz`; other trackers use schemes such as `E{NN}`, `S{NN}`, `I{NN}`, `T{NN}`), and location. Count totals per kind and, for sequential numbering schemes, detect numbering gaps.

### Hierarchy Validation

Analyze parent-child relationships across the hierarchy: epic to story, story to issue, issue to task. Identify orphaned items where parent references point to non-existent or archived records. Distinguish story-scoped issues from standalone issues. Verify task containment within parent issues.

### Status Distribution

Status breakdown per kind (the store's item vocabulary: open/in_progress/blocked/done/cancelled; other trackers may use draft/active/completed/blocked/cancelled or pending/in_progress/completed/blocked). Flag stale items: active with old creation dates, blocked without documented blockers. Compute completion rates.

### Dependency Mapping

Parent references in record state (`parent`). Cross-item references in content (context fields, dependency tables, notes). Task dependencies within issues. Blocking relationships governing execution order.

### Structural Patterns

Completion criteria conventions (checkbox lists vs prose). Context section completeness. Length distributions by type against schema guidelines. Anti-patterns: oversized tasks, missing criteria, empty context sections.

## Preferred Discovery Method

Use `ctxmod work list [--parent <id>]` for inventory and `ctxmod work show <id>` for individual items. The ctxmod work store keeps every record's state in its own state file; one CLI call replaces 3-10 manual tool calls. Reserve manual glob/read for:

- Operations not covered by `ctxmod work` commands
- Reading full work item body content (`ITEM.md`)
- Writing work item bodies (state changes go through `ctxmod work set`)
- Cases where the work store is unavailable

`work_store_first` in the ctxmod builtin `work_discipline` profile

## Discovery Methodology

### Phase 1: Broad Scan

Brief template for a procedure-depth agent (launched from objective depth):

```text
Perform a broad scan to build the complete work-item inventory across all directories.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`
```

**Preferred**: `ctxmod work list` returns the complete inventory with kind, status, parent, and identifier fields in a single call. Use `--kind` and `--status` filters to scope further.

**Fallback**: Glob item bodies (`items/*/*/ITEM.md` under the store directory `ctxmod state show` reports) and read the state files (`meta/*/*.yaml`) when the CLI is unavailable. Calculate inventory totals by type before detailed examination.

### Phase 2: Item Enumeration

**Preferred**: `ctxmod work show <id>` returns metadata, parent, owner, and status for each item; `ctxmod work list --parent <id>` and `--owner <id>` give children, reports, and intents.

**Fallback**: Read the state files of all discovered items. Extract: `id`, `kind`, `status`, `parent`, `created`, `issue_type` (issues, an unmanaged field), `owner`. Build inventory matrix indexed by kind and status. Validate identifier format against the store's ten-letter id grammar (or the tracker's `{TYPE}{NN}` convention).

### Phase 3: Relationship Analysis

Brief template for a procedure-depth agent (launched from objective depth):

```text
Detect structural patterns and validate parent-child relationships across the work-item hierarchy.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/pattern_detection.md`
```

**Application**: Validate every parent reference resolves to an existing item. Map dependency chains: epic to stories, story to issues, issue to tasks. Detect orphans (parent references non-existent paths), dangling references (content mentions absent identifiers), and circular parent chains.

### Phase 4: Pattern Extraction

Analyze structural conventions: completion criteria formatting, context section content, summary lengths. Identify outliers against schema guidelines. Catalog recurring phrasing and common section structures.

## Tool Permissions

**Allowed**: Glob (work-item directory scan), Grep (identifier and reference search), Read (state-file and content examination), Bash (read-only: `git log` for creation date context), Write (reserved report path only)

**Forbidden**: Modifying work-item files, launching sub-agents, writing outside the reserved report path, creating or deleting work items

## Report Structure

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the work-item discovery report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Work-item discovery reports include these objective-specific sections:

| Section | Content |
|---------|---------|
| Inventory Summary | Counts by type, identifier ranges, path locations |
| Hierarchy Analysis | Parent-child map, orphaned items with IDs, standalone vs story-scoped issues |
| Status Distribution | Breakdown by type, stale items with creation dates, completion rates |
| Dependency Map | Cross-references between items, parent validation results, blocking chains |
| Structural Patterns | Completion criteria conventions, length distributions, detected anti-patterns |
| Related Files | Work items examined during discovery |

Each finding includes `file:line` references to specific work-item locations.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Scanning all markdown files | Noise from non-work-item content | Restrict Glob to work-item directories |
| Skipping parent validation | Orphaned items go undetected | Verify every parent reference resolves |
| Ignoring identifier format | Miss numbering gaps and conventions | Check identifier format consistency (`{TYPE}{NN}` schemes, ten-letter store ids) |
| Flat status listing | No actionable prioritization | Categorize stale items by duration and severity |
| Shallow content scan | Miss implicit cross-references | Search for identifier patterns (record ids, `E01`/`I03`-style labels) in content |
| Claims without file references | Unverifiable findings | Include `file:line` for all reported items |

## Examples

**Orphan Detection**: `ctxmod work list --kind issue`, examine 40 issues. Find 3 orphaned issues whose `parent` names an archived story. Report paths and recommend parent reassignment or standalone conversion.

**Stale Item Audit**: Discover 80 work items. Cross-reference `created`/`updated` instants with current status. Identify 6 issues active 90+ days and 2 blocked without documented blockers. Report priority list for status review.

**Pattern Extraction**: Scan 60 issues and 25 tasks. Find 75% of issues use checkbox completion criteria while 25% use prose. Identify 12 tasks exceeding schema length guidelines (>250 words). Report distribution tables and outlier paths.

## Related

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/planning.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
`ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_structure_issue.md`
