---
description: "Compare alternatives for decision support"
binds_to_depth: main
---

# analyze_compare

Compare alternatives for decision support.

Usage:

```text
/analyze_compare [alternatives] [--criteria CRITERIA]
```

| Argument | Required | Description |
| --- | --- | --- |
| `alternatives` | yes | Comma-separated alternatives or scope pattern |
| `--criteria` | no | Explicit evaluation criteria (defaults to discovery) |

Work-item context: optional.

Launch the `analyze_compare_phase` agent with the command arguments in a brief that grants the `analyze_compare_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_analyze_compare_phase" />
<command arguments>
```
