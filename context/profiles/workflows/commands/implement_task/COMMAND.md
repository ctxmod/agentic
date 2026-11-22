---
description: "Execute work defined in a task work item"
binds_to_depth: main
---

# implement_task

Execute work defined in a task work item.

Usage:

```text
/implement_task [--task TASK_ID]
```

| Argument | Required | Description |
| --- | --- | --- |
| `--task` | yes | Task identifier (T01, T02, etc.) or resolved TASK_PATH |

Work-item context: required.

Launch the `implement_task_phase` agent with the command arguments in a brief that grants the `implement_task_phase` skill:

```text
<ctxmod_activate skill="implement_task_phase" />
<command arguments>
```
