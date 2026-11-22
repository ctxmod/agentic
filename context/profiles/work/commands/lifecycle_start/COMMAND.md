---
description: "Begin work on a work item and transition status to active"
binds_to_depth: main
---

# lifecycle_start

Begin work on a work item and transition status to active.

Usage:

```text
/lifecycle_start [work-item]
```

| Argument | Required | Description |
| --- | --- | --- |
| `work-item` | no | Work item record id or unique prefix (e.g. mnbvcxzqwr). Inferred from context if not provided. |

Work-item context: required.

Launch the `lifecycle_start_phase` agent with the command arguments in a brief that grants the `lifecycle_start_phase` skill:

```text
<ctxmod_activate skill="agentic_work_lifecycle_start_phase" />
<command arguments>
```
