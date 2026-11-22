# Work-Item Discovery Objective

Discover and catalog work items to map hierarchy integrity, status distribution, dependencies, and structural conventions. Produces a health assessment of work-item organization for planning and prioritization.

## Base Behavior

the `discovery_objective` skill

## Work-Item Categories

### Inventory Classification

Catalog work items by type (initiatives, deliverables, work items, atomic tasks), identifier format, and storage location. Count totals per type and detect identifier gaps. Recognize the tracking system's type taxonomy.

### Hierarchy Validation

Analyze parent-child relationships: initiatives to deliverables, deliverables to work items, work items to atomic tasks. Identify orphans where parent references point to non-existent entries. Distinguish scoped items (nested under a parent) from standalone items.

### Status Distribution

Compute status breakdown per item type using the tracking system's status model. Flag stale items: active with old dates, blocked without documented blockers, in progress beyond expected duration. Calculate completion rates per hierarchy level.

### Dependency Mapping

Extract cross-item references: parent fields, blocking relationships, linked items, and inline mentions of identifiers. Trace dependency chains to identify critical paths and execution ordering. Detect circular dependencies and unresolved references.

### Structural Patterns

Assess completion criteria conventions (checklists vs prose vs acceptance criteria fields). Evaluate description completeness and length distributions. Identify anti-patterns: oversized tasks lacking decomposition, missing acceptance criteria, empty descriptions.

## Preferred Discovery Method

When the tracking system is the ctxmod work store, use `ctxmod work list [--parent <id>]` for inventory and `ctxmod work show <id>` for individual items. The store answers from its state files in a single CLI call. Reserve manual scanning for:

- Tracking systems other than the ctxmod work store
- Operations not covered by `ctxmod work` commands
- Reading full item body content beyond what `ctxmod work show` returns
- Writing item bodies (state changes go through `ctxmod work set`)

`work_store_first` in the ctxmod builtin `work_discipline` profile

## Discovery Methodology

### Phase 1: Broad Scan

Brief template for a procedure-depth agent (launched from objective depth):

```text
Perform a broad scan to build the complete work-item inventory from the tracking system's storage.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`
```

**Preferred (work-store items)**: `ctxmod work list` returns the complete inventory with metadata in one call (paged; `--page-size` to widen).

**Fallback (other systems or an unavailable work store)**: Query or scan work-item storage locations to build a complete inventory. Use system-appropriate methods: file globbing for file-based trackers, API queries for hosted systems, CLI tools for integrated platforms. Calculate totals by type before detailed examination.

### Phase 2: Item Enumeration

**Preferred (work-store items)**: `ctxmod work show <id>` returns identifier, kind, status, parent, owner, title, body path, and every other state field per item; `ctxmod work list --parent <id>` lists children and `ctxmod work list --owner <id> --kind report,intent` the owned reports and intents.

**Fallback**: Read metadata from all discovered items. Extract: identifier, type, status, parent reference, creation date, assignee, and system-specific fields. Build an inventory matrix indexed by type and status. Validate identifier formats against project conventions.

### Phase 3: Relationship Analysis

Brief template for a procedure-depth agent (launched from objective depth):

```text
Detect structural patterns and validate parent-child relationships across the work-item hierarchy.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/pattern_detection.md`
```

**Application**: Validate every parent reference resolves to an existing item. Map dependency chains across hierarchy levels. Detect orphans (parent references non-existent items), dangling references (mentions of absent identifiers), and circular dependencies.

### Phase 4: Pattern Extraction

Analyze structural conventions: completion criteria formatting, description patterns, field completeness. Identify outliers against project guidelines. Catalog recurring structures and deviations.

## Tool Permissions

**Allowed**: Glob (work-item file scan), Grep (identifier and reference search), Read (metadata and content examination), Bash (read-only: `git log` for date context, CLI queries to tracking systems), Write (reserved report path only)

**Forbidden**: Modifying work items, launching sub-agents, writing outside the reserved report path, creating or deleting items

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
| Inventory Summary | Counts by type, identifier ranges, storage locations |
| Hierarchy Analysis | Parent-child map, orphaned items with identifiers, scoped vs standalone items |
| Status Distribution | Breakdown per type, stale items with dates, completion rates per level |
| Dependency Map | Cross-references between items, parent validation results, blocking chains |
| Structural Patterns | Completion criteria conventions, length distributions, detected anti-patterns |
| Related Items | Work items examined during discovery with location references |

Each finding includes references to specific work-item locations.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Scanning all project files | Noise from non-work-item content | Restrict search to work-item storage locations |
| Skipping parent validation | Orphaned items go undetected | Verify every parent reference resolves |
| Ignoring identifier conventions | Miss numbering gaps and format drift | Check identifier format consistency |
| Flat status listing | No actionable prioritization | Categorize stale items by duration and severity |
| Shallow content scan | Miss implicit cross-references | Search for identifier patterns in item descriptions |
| Claims without location references | Unverifiable findings | Include specific references for all reported items |

## Examples

**Jira-Style Hierarchy**: Query project `{PROJECT-KEY}`, enumerate 120 items across epics, stories, tasks, and sub-tasks. Build hierarchy from `parent` and `epic link` fields. Detect 5 orphaned stories referencing deleted epic `{PROJECT-KEY}-42`. Report 3 blocked stories without linked blockers. Recommend reassignment.

**GitHub Issues with Projects**: Scan issues and project boards via `gh` CLI. Catalog 85 open issues across 3 milestones. Map issue-to-project associations and label categorization. Find 12 issues assigned to closed milestones, 8 with `blocked` label but no linked blocker. Report milestone completion rates and stale item list.

**Azure DevOps with Area Paths**: Query items under area path `{ORGANIZATION}\{PROJECT}\{AREA}`. Enumerate 200 items across epics, features, user stories, tasks, and bugs. Validate parent-child links per level. Detect 4 user stories with iteration paths mismatched from parent feature. Report hierarchy integrity score and distribution table.

## Related

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery_objective/reference/work_items.md" />
