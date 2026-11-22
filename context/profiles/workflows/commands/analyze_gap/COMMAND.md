---
description: "Identify missing elements relative to requirements or target state"
binds_to_depth: main
---

# analyze_gap

Identify missing elements relative to requirements or target state.

Usage:

```text
/analyze_gap [scope] [--input INPUT_REPORT] [--target TARGET_SPEC]
```

| Argument | Required | Description |
| --- | --- | --- |
| `scope` | no | Target scope or path for analysis |
| `--input` | no | Explicit input report path (skips discovery) |
| `--target` | no | Target specification to compare against |

Work-item context: optional.

Launch the `analyze_gap_phase` agent with the command arguments in a brief that grants the `analyze_gap_phase` skill:

```text
<ctxmod_activate skill="analyze_gap_phase" />
<command arguments>
```
