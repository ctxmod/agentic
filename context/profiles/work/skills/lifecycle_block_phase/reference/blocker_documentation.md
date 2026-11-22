# Blocker Documentation

Use when documenting a blocking condition and transitioning a work item to blocked status. Combines status update and blocker section modification in a single atomic operation.

Brief template for a sub-agent (model: haiku):

```text
Document blocker and update status to blocked for {WORK_ITEM_ID}.

Compulsory reads:
- `{VERIFICATION_REPORT}`

Parameters:
- ARTIFACT_PATH={WORK_ITEM_PATH}/{TYPE}.md
- BLOCKER_REASON={reason}
- BLOCK_DATE={date}
- VERIFICATION_REPORT={block-verification report path}

Delimitations:
- Update status to 'blocked'
- Add or update Blockers section
- Do not modify other content

Notes:
- Read work item at {WORK_ITEM_PATH}/{TYPE}.md
- Change status from 'active' to 'blocked'
- Add blocker entry with reason and date
- Preserve existing blockers if section exists
```
