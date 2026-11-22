---
description: "Multi-dimensional quality assessment of artifacts"
binds_to_depth: main
---

# analyze_quality

Multi-dimensional quality assessment of artifacts.

Usage:

```text
/analyze_quality [scope] [--dimensions DIMENSIONS]
```

| Argument | Required | Description |
| --- | --- | --- |
| `scope` | yes | Target scope or path for quality assessment |
| `--dimensions` | no | Comma-separated quality dimensions (defaults to standard set) |

Work-item context: optional.

Launch the `analyze_quality_phase` agent with the command arguments in a brief that grants the `analyze_quality_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_analyze_quality_phase" />
<command arguments>
```
