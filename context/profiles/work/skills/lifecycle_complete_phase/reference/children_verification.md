# Children Verification

Use when verifying that all child items are resolved (completed or cancelled) before completing a parent work item. Paired with criteria verification for parallel completion assessment.

Preferred: `ctxmod work list --parent <id>` for initial child enumeration (kind, status, title, file). Full file read still needed for criteria verification. See `work_store_first` in the ctxmod builtin `work_discipline` profile.

Brief template for a sub-agent (model: sonnet):

```text
Verify child item resolution status for {WORK_ITEM_ID}.

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "completion-children" --owner <work item, if any>` immediately before writing; returns the path
- WORK_ITEM_PATH={work item directory}
- WORK_ITEM_TYPE={epic|story|issue|task}

Delimitations:
- Verify child item status only
- Do not verify acceptance criteria

Notes:
- Read work item at {WORK_ITEM_PATH}/{TYPE}.md
- List all referenced child items
- Check status of each (completed or cancelled)
- Document any unresolved children
```

## Model Override

Uses Sonnet instead of the lifecycle default. Children verification requires traversing hierarchy and assessing resolution status across multiple items.
