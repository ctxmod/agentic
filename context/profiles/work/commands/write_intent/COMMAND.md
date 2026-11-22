---
description: "Create or edit intent files with clarification, research, formatting, and improvement suggestions"
binds_to_depth: main
---

# write_intent

Create or edit intent files with clarification, research, formatting, and improvement suggestions.

Usage:

```text
/write_intent [description] | /write_intent --edit <path>
```

| Argument | Required | Description |
| --- | --- | --- |
| `description` | no | Intent description text or reference to attached file (required unless --edit provided) |
| `--edit <path>` | no | Edit existing intent file at specified path |
| `-e, --epic <id>` | no | Epic identifier for work-item scoping |
| `-s, --story <id>` | no | Story identifier for work-item scoping |
| `-i, --issue <id>` | no | Issue identifier for work-item scoping |
| `-t, --task <id>` | no | Task identifier for work-item scoping |

Work-item context: optional.

Run the `intent_write` skill directly in this session with the command arguments.
