---
description: "Apply corrections identified by validation reports"
binds_to_depth: main
---

# implement_fix

Apply corrections identified by validation reports.

Usage:

```text
/implement_fix [--input INPUT_REPORT] [--severity LEVEL]
```

| Argument | Required | Description |
| --- | --- | --- |
| `--input` | no | Validation report path (uses most recent if omitted) |
| `--severity` | no | Minimum severity to fix: `error`, `warning`, or `info` (default: `warning`) |

Work-item context: optional.

Launch the `implement_fix_phase` agent with the command arguments in a brief that grants the `implement_fix_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_implement_fix_phase" />
<command arguments>
```
