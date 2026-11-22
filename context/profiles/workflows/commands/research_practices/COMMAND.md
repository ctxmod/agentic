---
description: "Research current best practices for a topic from external sources"
binds_to_depth: main
---

# research_practices

Research current best practices for a topic from external sources.

Usage:

```text
/research_practices [topic]
```

| Argument | Required | Description |
| --- | --- | --- |
| `topic` | yes | Topic or question to research best practices for |

Work-item context: optional.

Launch the `research_practices_phase` agent with the command arguments in a brief that grants the `research_practices_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_research_practices_phase" />
<command arguments>
```
