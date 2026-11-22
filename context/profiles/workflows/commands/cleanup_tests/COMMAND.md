---
description: "Analyze and remove redundant, implementation-pinning, flaky, and low-value tests"
binds_to_depth: main
---

# cleanup_tests

Analyze and remove redundant, implementation-pinning, flaky, and low-value tests.

Usage:

```text
/cleanup_tests [scope]
```

| Argument | Required | Description |
| --- | --- | --- |
| `scope` | no | Path or package to scope the sweep (defaults to repo root) |

Work-item context: optional.

Launch the `cleanup_tests_phase` agent with the command arguments in a brief that grants the `cleanup_tests_phase` skill:

```text
<ctxmod_activate skill="cleanup_tests_phase" />
<command arguments>
```
