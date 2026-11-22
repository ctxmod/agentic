---
description: "Archive completed or stale work items and reports in the ctxmod work store (preview first, move-only, never deletes)"
binds_to_depth: main
---

# archive_cleanup

Archive completed or stale work items and reports in the ctxmod work store (preview first, move-only, never deletes).

Usage:

```text
/archive_cleanup [filter]
```

| Argument | Required | Description |
| --- | --- | --- |
| `filter` | no | Work item id, short id, or path to scope the archive. Empty archives every item subject to filters. |

Work-item context: optional.

Launch the `archive_cleanup_phase` agent with the command arguments in a brief that grants the `archive_cleanup_phase` skill:

```text
<ctxmod_activate skill="archive_cleanup_phase" />
<command arguments>
```
