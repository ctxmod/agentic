# Criteria Verification

Use when verifying that a work item's acceptance or success criteria are satisfied before completing. Paired with children verification for parallel completion assessment.

Preferred: `ctxmod work show <id>` for metadata validation (id, status, kind). Full file read still required for criteria text. See `work_store_first` in the ctxmod builtin `work_discipline` profile.

Brief template for a sub-agent (model: sonnet):

```text
Verify acceptance criteria are satisfied for {WORK_ITEM_ID}.

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "completion-criteria" --owner <work item, if any>` immediately before writing; returns the path
- WORK_ITEM_PATH={work item directory}
- WORK_ITEM_TYPE={epic|story|issue|task}

Delimitations:
- Verify acceptance/success criteria only
- Do not verify child item status

Notes:
- Read work item at {WORK_ITEM_PATH}/ITEM.md (status from `ctxmod work show`)
- Check each criterion in Success Criteria section
- Determine verification status for each
- Document any unmet criteria
```

## Model Override

Uses Sonnet instead of the lifecycle default. Criteria verification requires reasoning about whether evidence satisfies each acceptance criterion.
