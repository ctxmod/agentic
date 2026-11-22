---
description: "Find all references and usages of target artifacts"
binds_to_depth: main
---

# discover_consumers

Find all references and usages of target artifacts.

Usage:

```text
/discover_consumers <target> [--scope SCOPE]
```

| Argument | Required | Description |
| --- | --- | --- |
| `target` | yes | Target artifact, pattern, or identifier to trace |
| `--scope` | no | Search scope (defaults to project root) |

Work-item context: optional.

Launch the `discover_consumers_phase` agent with the command arguments in a brief that grants the `discover_consumers_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_discover_consumers_phase" />
<command arguments>
```
