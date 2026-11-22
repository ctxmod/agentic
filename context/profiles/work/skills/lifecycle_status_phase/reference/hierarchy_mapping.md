# Hierarchy Mapping

Use when mapping the full relationship graph for a specific work item. Traces parent chain to epic level, lists children with statuses, and identifies siblings.

Preferred: `ctxmod work show <id>` (and its parent's, walking up) gives the parent chain; `ctxmod work list --parent <id>` gives children with statuses, and `ctxmod work list --parent <parent id>` the siblings. Keep manual traversal as fallback. See `work_store_first` in the ctxmod builtin `work_discipline` profile.

Brief template for a sub-agent (model: haiku):

```text
Map work item hierarchy relationships for {WORK_ITEM_ID}.

Compulsory reads:
- `{STATUS_REPORT}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "hierarchy" --owner <work item, if any>` immediately before writing; returns the path
- STATUS_REPORT={status gathering report path}

Delimitations:
- Map relationships only
- Do not assess progress

Notes:
- Read status report for parent/child context
- Trace parent chain to epic level
- List all children with status
- Identify sibling items
```
