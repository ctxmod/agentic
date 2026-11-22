---
description: "Map structural organization, boundaries, and component relationships"
binds_to_depth: main
---

# discover_architecture

Map structural organization, boundaries, and component relationships.

Usage:

```text
/discover_architecture [scope]
```

| Argument | Required | Description |
| --- | --- | --- |
| `scope` | no | Path or glob pattern (defaults to project root) |

Work-item context: optional.

Launch the `discover_architecture_phase` agent with the command arguments in a brief that grants the `discover_architecture_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_discover_architecture_phase" />
<command arguments>
```
