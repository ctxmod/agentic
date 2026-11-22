---
description: "Solidify durable memories from the work store into the context module's reference, then retire superseded memories (verify-before-retire gate; manual-invocation only)"
binds_to_depth: main
---

# cleanup_memory

Solidify durable memories from the work store into the context module's reference, then retire superseded memories (verify-before-retire gate; manual-invocation only).

Usage:

```text
/cleanup_memory [filter]
```

| Argument | Required | Description |
| --- | --- | --- |
| `filter` | no | Memory name (slug), name prefix, or path to scope the sweep. Empty analyzes every memory record in the work store. |

Work-item context: optional.

Launch the `cleanup_memory_phase` agent with the command arguments in a brief that grants the `cleanup_memory_phase` skill:

```text
<ctxmod_activate skill="agentic_work_cleanup_memory_phase" />
<command arguments>
```
