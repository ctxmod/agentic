---
description: "Verify completion criteria and mark work item as completed"
binds_to_depth: main
---

# lifecycle_complete

Verify completion criteria and mark work item as completed.

Usage:

```text
/lifecycle_complete [work-item]
```

| Argument | Required | Description |
| --- | --- | --- |
| `work-item` | no | Work item record id or unique prefix (e.g. mnbvcxzqwr). Inferred from context if not provided. |

Work-item context: required.

Launch the `lifecycle_complete_phase` agent with the command arguments in a brief that grants the `lifecycle_complete_phase` skill:

```text
<ctxmod_activate skill="lifecycle_complete_phase" />
<command arguments>
```
