# Context Resolution Phase

Determine the work item context from available information sources. This phase establishes which work item a command operates on.

## When to Use

Use context resolution phase when:

- A command requires work item context to operate
- Work item reference may be ambiguous (parameter, conversation, or directory)
- Main agent needs to establish the work item (record id and item directory) before launching sub-agents
- Commands accept optional work item identifiers

Context resolution is the first phase for all lifecycle commands.

## Main Agent Behavior

This phase executes directly within the main agent. No sub-agent launch required.

### Resolution Order

Work item context resolves in priority order:

1. **Explicit parameter**: `/... mnbvcxzqwr` (a record id, or a unique prefix of at least four letters) → Use that record
2. **Active conversation context**: Work item mentioned in recent conversation
3. **Current directory**: Directory location within the store's `items/` tree (an item directory's entry name carries its id: `<DD-HHMM>-<id>-<slug>`)
4. **Error**: If unresolved after all attempts

### Resolution Procedure

Preferred: `ctxmod work show <id>` returns the record's kind, status, parent, and `file` (its `ITEM.md`) in one call, and refuses an id that names no record (`CTX-WRK004`) or a prefix two records share (`CTX-WRK005`). Retain the `find`-based resolution below as active fallback (this is a safety-critical phase).

`work_store_first` in the ctxmod builtin `work_discipline` profile

**For explicit parameter:**

```bash
# Preferred: the store resolves ids and unique prefixes
ctxmod work show "$IDENTIFIER"          # kind, status, parent, file=<item dir>/ITEM.md

# Fallback: validate the id grammar, then find the item directory by its entry name
STORE=<store directory from `ctxmod state show`>
if [[ "$IDENTIFIER" =~ ^[bcdfghjkmnpqrstvwxz]{4,10}$ ]]; then
  WORK_ITEM_PATH=$(find "$STORE/items" -mindepth 2 -maxdepth 2 -type d -name "*-${IDENTIFIER}*" | head -1)
fi
```

**For directory context:**

```bash
# Extract the work item from the current working directory (an item directory under the store)
WORK_ITEM_PATH=$(pwd | grep -oE 'items/[0-9]{4}-[0-9]{2}/[0-9]{2}-[0-9]{4}-[bcdfghjkmnpqrstvwxz]{10}[^/]*')
```

### Validation

After resolution, validate:

- The record exists (`ctxmod work show` succeeds) and is not archived
- Its body exists at `<WORK_ITEM_PATH>/ITEM.md`
- The record carries the required fields (id, title, status)

If validation fails, report error with guidance to specify work item explicitly.

## Output

This phase produces parameters for subsequent phases:

| Parameter | Value | Example |
|-----------|-------|---------|
| WORK_ITEM_PATH | Resolved work item directory | `context.local/work/items/2026-02/22-0830-mnbvcxzqwr-token-refresh` |
| WORK_ITEM_TYPE | Kind from the record | `issue` |
| WORK_ITEM_ID | Record id | `mnbvcxzqwr` |

## Context Types

| Context Level | Parameters Available |
|---------------|----------------------|
| Epic | EPIC_PATH |
| Story | STORY_PATH (may include EPIC_PATH if nested) |
| Issue | ISSUE_PATH (may include STORY_PATH, EPIC_PATH if nested) |
| Task | TASK_PATH (may include all parent paths) |

Main agent provides only the parameters for active context levels.

## Transition Criteria

Context resolution completes when:

- Work item path is resolved and validated
- Work item type is determined
- Required parameters are available for subsequent phases

If context cannot be resolved, halt with error and provide guidance.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Proceeding without validation | Commands operate on wrong context | Always validate resolved path exists |
| Assuming single resolution source | Fails when parameter omitted | Check all sources in priority order |
| Silently ignoring resolution failures | Commands fail later with unclear cause | Report error immediately with guidance |
| Hardcoding path assumptions | Breaks when hierarchy varies | Use dynamic resolution from identifiers |
