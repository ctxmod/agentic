---
description: "Extract recurring patterns and conventions from artifacts"
binds_to_depth: main
---

# discover_patterns

Extract recurring patterns and conventions from artifacts.

Usage:

```text
/discover_patterns [scope] [--type PATTERN_TYPE]
```

| Argument | Required | Description |
| --- | --- | --- |
| `scope` | no | Path or glob pattern (defaults to project root) |
| `--type` | no | Pattern type filter: structural, naming, organizational, behavioral, or all (default: all) |

Work-item context: optional.

Launch the `discover_patterns_phase` agent with the command arguments in a brief that grants the `discover_patterns_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_discover_patterns_phase" />
<command arguments>
```
