---
description: "Map component relationships and dependency structures"
binds_to_depth: main
---

# discover_dependencies

Map component relationships and dependency structures.

Usage:

```text
/discover_dependencies [scope] [--depth DEPTH]
```

| Argument | Required | Description |
| --- | --- | --- |
| `scope` | no | Path or glob pattern (defaults to project root) |
| `--depth` | no | Dependency depth: direct, transitive, or all (default: direct) |

Work-item context: optional.

Launch the `discover_dependencies_phase` agent with the command arguments in a brief that grants the `discover_dependencies_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_discover_dependencies_phase" />
<command arguments>
```
