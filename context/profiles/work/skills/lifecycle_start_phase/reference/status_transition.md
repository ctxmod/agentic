# Status Transition Phase

Update the work item record's status to reflect lifecycle state changes (`ctxmod work set <id> status=<target>`). This phase handles status updates exclusively; the store changes only the named field and never touches the body.

## When to Use

Use status transition phase when:

- Work item lifecycle status changes (`open` → `in_progress` → `done`, `in_progress` → `blocked`, etc.)
- Status transition has been verified as eligible
- Only the status requires changing
- Transition is part of a lifecycle command workflow

Status transition follows verification phases and precedes notification.

Brief template for a sub-agent (model: haiku):

```text
Transition {work item} status from {current} to {target}.

Parameters:
- WORK_ITEM_ID={record id}
- CURRENT_STATUS={current status}
- CURRENT_REV={record revision from `ctxmod work show`}
- TARGET_STATUS={target status}
- VERIFICATION_REPORT={verification report path}

Command:
- ctxmod work set {WORK_ITEM_ID} status={TARGET_STATUS} --if-rev {CURRENT_REV}

Delimitations:
- Change the status field only
- Do not set other fields
- Do not modify the body (ITEM.md)

Notes:
- `--if-rev` refuses (CTX-WRK019) if the record changed since CURRENT_STATUS was read — re-read and report instead of retrying blindly
- An archived record refuses every change (CTX-WRK013)
```

## Transition Types

| Transition | From | To | Verification Required |
|------------|------|-----|-----------------------|
| Start | `open` | `in_progress` | Required sections present, parent `in_progress`/`done` |
| Start (unblock) | `blocked` | `in_progress` | Blocker resolved, resolution documented |
| Block | `in_progress` | `blocked` | Blocker reason documented |
| Complete | `in_progress` | `done` | Criteria met, children resolved |
| Cancel | any | `cancelled` | Cancellation reason documented |

Each transition type requires prior verification of transition-specific criteria. A start takes the item with `ctxmod work claim <item> --status in_progress`, one change that refuses while another session holds the item; a completion or cancellation sets the status, then releases the claim (`ctxmod work release <item>`). A blocked item keeps its claim unless its holder releases it; release it when someone else will pick it up.

## Context Requirements

**Parameters provided by main agent**:

| Parameter | Required | Purpose |
|-----------|----------|---------|
| WORK_ITEM_ID | Yes | Work item record to modify |
| CURRENT_STATUS | Yes | Expected current status (for validation) |
| TARGET_STATUS | Yes | New status value to set |
| VERIFICATION_REPORT | Yes | Verification report confirming eligibility |

## Parent Status Automation

When transitioning a work item that has a parent in `open` (draft) status, the main agent must transition the parent to `in_progress` (active) first. This ensures the hierarchy remains consistent.

**Automation sequence:**

1. Check if the work item has a parent (the record's `parent` field, `ctxmod work show`)
2. If parent exists and parent status is `open` (draft):
   - Launch status transition for parent (`open` → `in_progress`)
   - Wait for parent transition to complete
3. Proceed with child work item transition

This automation applies to all transitions except cancellation.

## Output Format

Status transition produces the modified record state (a new revision of its state file). No separate report generated.

Sub-agent response:

```
Transitioned {work item ID} status from {current} to {target}.

Summary: Set status on record {WORK_ITEM_ID} (rev {new rev}). No other field and no body changed.
```

## Transition Criteria

Status transition completes when:

- The record's status reads the target value (`ctxmod work show`)
- No other field changed (`ctxmod work history <id>` shows a status-only change)
- The body is untouched

If transition fails, report the issue with failure context.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Transitioning without verification | Invalid state changes | Always verify eligibility first |
| Modifying content sections | Exceeds phase scope | Change the record's status only |
| Skipping parent automation | Hierarchy inconsistencies | Check and transition parents first |
| Using Sonnet/Opus | Wastes capability on simple task | Use Haiku for field updates |
| Setting a status outside the vocabulary | Refused (CTX-WRK006) | Use only defined transition types |
