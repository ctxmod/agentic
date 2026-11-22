---
description: "Create synthesis report combining findings from multiple sources"
binds_to_depth: main
---

# writing_report

Create synthesis report combining findings from multiple sources.

Usage:

```text
/writing_report [type] [description]
```

| Argument | Required | Description |
| --- | --- | --- |
| `type` | yes | Report type (e.g. analysis, discovery, validation, summary) |
| `description` | yes | Brief description of report scope |

Work-item context: optional.

Launch the `write_report_phase` agent with the command arguments in a brief that grants the `write_report_phase` skill:

```text
<ctxmod_activate skill="write_report_phase" />
<command arguments>
```
