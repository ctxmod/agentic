---
description: "Comprehensive quality audit through multi-criteria parallel validation and synthesis"
binds_to_depth: main
---

# review_audit

Comprehensive quality audit through multi-criteria parallel validation and synthesis.

Usage:

```text
/review_audit [scope]
```

| Argument | Required | Description |
| --- | --- | --- |
| `scope` | no | Target scope for audit (glob pattern, directory, or omit for current context) |

Work-item context: optional.

Launch the `review_audit_phase` agent with the command arguments in a brief that grants the `review_audit_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_review_audit_phase" />
<command arguments>
```
