---
description: "Remove compat shims, reference-preserving duplicates, and DRY/SSOT violations from shipping code (verify-before-remove gate; manual-invocation only)"
binds_to_depth: main
---

# cleanup_code_debt

Remove compat shims, reference-preserving duplicates, and DRY/SSOT violations from shipping code (verify-before-remove gate; manual-invocation only).

Usage:

```text
/cleanup_code_debt [target]
```

| Argument | Required | Description |
| --- | --- | --- |
| `target` | no | File path, symbol name, or scope to analyze. Empty discovers candidates via the code index and lexical-search heuristics. |

Work-item context: optional.

Launch the `cleanup_code_debt_phase` agent with the command arguments in a brief that grants the `cleanup_code_debt_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_cleanup_code_debt_phase" />
<command arguments>
```
