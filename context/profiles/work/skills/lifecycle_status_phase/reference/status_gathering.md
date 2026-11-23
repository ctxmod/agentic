# Status Gathering

Use when gathering comprehensive status information for a specific work item. Extracts status from the work record, lists children with statuses, notes blockers, and identifies parent context.

Preferred: `ctxmod work show <id>` returns status, blocker fields, and parent; `ctxmod work list --parent <id>` returns the children with their statuses. Keep manual read + frontmatter parse as fallback. See `work_store_first` in the ctxmod builtin `work_discipline` profile.

Brief template for a sub-agent (model: haiku):

```text
Gather work item status and context information for {WORK_ITEM_ID}.

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "status" --owner <work item, if any>` immediately before writing; returns the path
- WORK_ITEM_PATH={work item directory}
- WORK_ITEM_TYPE={epic|story|issue|task}

Delimitations:
- Discovery only; do not modify anything
- Focus on status-relevant information

Notes:
- Read work item at {WORK_ITEM_PATH}/ITEM.md (status, parent and blocker fields from `ctxmod work show`)
- Extract current status from the record (`ctxmod work show`)
- List child items with their statuses
- Note any blockers
- Identify parent context
```
