---
description: "Display current work item context and status overview"
binds_to_depth: main
---

# lifecycle_status

Display current work item context and status overview.

Usage:

```text
/lifecycle_status [work-item]
```

| Argument | Required | Description |
| --- | --- | --- |
| `work-item` | no | Work item record id or unique prefix (e.g. mnbvcxzqwr). Shows general context if not provided. |

Work-item context: optional.

Launch the `lifecycle_status_phase` agent with the command arguments in a brief that grants the `lifecycle_status_phase` skill:

```text
<ctxmod_activate skill="lifecycle_status_phase" />
<command arguments>
```
