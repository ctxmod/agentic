---
description: "Technical debt inventory and prioritization"
binds_to_depth: main
---

# analyze_debt

Technical debt inventory and prioritization.

Usage:

```text
/analyze_debt [scope] [--categories CATEGORIES]
```

| Argument | Required | Description |
| --- | --- | --- |
| `scope` | no | Target scope or path (defaults to full project) |
| `--categories` | no | Comma-separated debt categories to focus on |

Work-item context: optional.

Launch the `analyze_debt_phase` agent with the command arguments in a brief that grants the `analyze_debt_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_analyze_debt_phase" />
<command arguments>
```
