# Work Item Survey

Use when no specific work item context exists and a general survey of project work items is needed. Inventories active and blocked items grouped by type.

Preferred: `ctxmod work list --kind epic,story,issue,task --status open,in_progress,blocked` for inventory and `ctxmod work status` for counts. Keep glob patterns as fallback when the work store is unavailable. See `work_store_first` in the ctxmod builtin `work_discipline` profile.

Brief template for a sub-agent (model: haiku):

```text
Survey active work items across the project.

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE=the store's item bodies (`<store>/items/*/*/ITEM.md`; `ctxmod state show` names the store)

Delimitations:
- Discovery only; inventory active items
- Do not analyze progress deeply

Notes:
- Find work items with status 'active' or 'blocked'
- Group by type (epic, story, issue, task)
- Note blocked items and their reasons
```
