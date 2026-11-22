# Report Organization Procedure

Determine correct report placement in directory structure based on work context, report type, and temporal factors.

## Prerequisites

- Work context identified (general, epic, story, issue, task, or custom)
- Report type determined (discovery, analysis, validation, summary, implementation, remediation)
- Current date and time available (via the host clock (`date -u`))

## Steps

Inside the ctxmod work store, `ctxmod work new report "<title>" [--owner <item id>]`
performs steps 1–4 itself: the owner decides the base path (the owning
item's directory, or the store's top level), the store applies the
month directory (`reports/<YYYY-MM>/`), builds the entry name
(`<DD-HHMM>-<id>-<slug>.md`, slug derived from the title), and
guarantees availability by minting a fresh id. The steps below
describe the same decisions for reports kept outside the store.

### 1. Identify Base Path

Determine root directory from work context:

| Context | Base Path Pattern |
|---------|-------------------|
| General work | `{reports_root}` |
| Named initiative | `{reports_root}/{initiative-slug}` |
| Specific item | `{reports_root}/{item-id}` |
| Nested context | `{parent_path}/{child-id}/reports` |

Base path depends on project conventions. When no convention exists, use a dedicated reports directory at project root.

### 2. Apply Date Directory

Add date directory under base path:

```
{base_path}/{YYYY-MM-DD}/
```

Date directories group reports temporally, enabling:
- Chronological navigation
- Cleanup of old reports
- Context reconstruction for specific periods

### 3. Construct File Name

Build file name from components:

```
{HH-mm}-{type}-{slug}.md
```

| Component | Source | Example |
|-----------|--------|---------|
| Time prefix | Report creation time | `{time}` |
| Type | Report classification | `discovery`, `validation` |
| Slug | Content descriptor | `auth-module`, `api-review` |

Time prefix ensures uniqueness when multiple reports of same type created daily.

### 4. Verify Path Availability

Before creation, check:

- Parent directories exist (create if needed)
- File name not already taken
- Path length within system limits

If file exists, append counter: `{time}-discovery-auth-module-2.md`

### 5. Record Path in Context

Record the report path for:
- Subsequent phases referencing this output
- Summary generation across reports
- Cleanup and archival processes

Report paths enter registration records only as returned paths: the
writing agent reserved its own path just-in-time by creating the report record
(`ctxmod work new report "<title>"`),
wrote to the printed `<DD-HHMM>-<id>-<slug>.md` path, and returned that path.
Never pre-register a report path as a destination handed to an agent
before work starts. Registration mechanisms remain system-specific
(file listing, manifest update, or parameter passing) and capture the
returned path for downstream consumers.

## Decision Points

**No established convention**: Create `reports/` at project root. Use flat structure initially; add hierarchy when report volume warrants.

**Multiple related reports**: Place in shared date directory with distinguishing slugs. Avoid deep nesting for related reports—keep them discoverable.

**Report updates existing content**: Use new timestamp rather than overwriting. Previous reports remain available for comparison and audit.

**Cross-context reports**: When report spans multiple items, place at common ancestor level or general reports directory. Reference specific items in metadata.

## Directory Structure Examples

**Simple**: `reports/{date}/{time}-{type}-{slug}.md`

**Structured**: `reports/{context}/{item-id}/{date}/{time}-{type}-{slug}.md`

## Success Criteria

- Path follows established convention or creates sensible default
- Date directory applied for temporal grouping
- File name includes time, type, and descriptor
- No overwriting of existing reports
- Path recorded for downstream consumers

## Common Issues

**Path too long**: Deep nesting exceeds filesystem limits. Flatten structure; use IDs rather than full names in paths.

**Timestamp collision**: Rapid report generation creates identical timestamps. Use second precision or append counter.

**Orphaned reports**: Reports created outside established structure. Audit periodically; move orphans to proper location or document exception.

**Convention drift**: Different phases use different conventions. Establish single source of truth for path patterns; validate during report creation.

**Stale date directories**: Old reports accumulate. Implement retention policy; archive or delete reports beyond retention period.
