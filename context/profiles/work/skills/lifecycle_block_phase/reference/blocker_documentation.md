# Blocker Documentation

Use when documenting a blocking condition and transitioning a work item to blocked status. Combines the status change (a record field, `ctxmod work set`) and the Blockers section of the body in one sub-agent step.

Brief template for a sub-agent (model: haiku):

```text
Document blocker and update status to blocked for {WORK_ITEM_ID}.

Compulsory reads:
- `{VERIFICATION_REPORT}`

Parameters:
- ARTIFACT_PATH={WORK_ITEM_PATH}/ITEM.md
- BLOCKER_REASON={reason}
- BLOCK_DATE={date}
- VERIFICATION_REPORT={block-verification report path}

Delimitations:
- Update status to 'blocked'
- Add or update Blockers section
- Do not modify other content

Notes:
- Read work item at {WORK_ITEM_PATH}/ITEM.md
- Change status from 'in_progress' to 'blocked' with `ctxmod work set <item> status=blocked`
- Add blocker entry with reason and date
- Preserve existing blockers if section exists
```
