---
description: "Mark work item as blocked with documented reason"
binds_to_depth: main
---

# lifecycle_block

Mark work item as blocked with documented reason.

Usage:

```text
/lifecycle_block [work-item] [--reason REASON]
```

| Argument | Required | Description |
| --- | --- | --- |
| `work-item` | no | Work item record id or unique prefix (e.g. mnbvcxzqwr). Inferred from context if not provided. |
| `--reason` | yes | Brief description of blocking condition |

Work-item context: required.

Launch the `lifecycle_block_phase` agent with the command arguments in a brief that grants the `lifecycle_block_phase` skill:

```text
<ctxmod_activate skill="agentic_work_lifecycle_block_phase" />
<command arguments>
```
