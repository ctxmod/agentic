---
description: "Reorganize artifacts without changing behavior"
binds_to_depth: main
---

# implement_refactor

Reorganize artifacts without changing behavior.

Usage:

```text
/implement_refactor [scope] [--type REFACTOR_TYPE]
```

| Argument | Required | Description |
| --- | --- | --- |
| `scope` | yes | Target scope: path, glob pattern, or artifact reference |
| `--type` | no | Refactor type: `rename`, `extract`, `inline`, `move`, `restructure`, or `consolidate` |

Work-item context: required.

Launch the `implement_refactor_phase` agent with the command arguments in a brief that grants the `implement_refactor_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_implement_refactor_phase" />
<command arguments>
```
