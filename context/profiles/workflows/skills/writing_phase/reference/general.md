# General Writing

Use when producing a written artifact where prose quality matters. Always uses Opus. Suitable for most writing needs where specialized work item, report, or specification patterns are unnecessary.

Brief template for a sub-agent (model: opus):

```text
Write {artifact type}: {brief description}

Compulsory reads:
- the `writing_objective` skill
- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`
- `{relevant schema for artifact type}`
- `{INPUT_REPORT from prior phase if applicable}`

Parameters:
- OUTPUT_PATH={path where artifact should be written}
- {additional context parameters}

Delimitations:
- Write only: specified artifact
- Follow schema exactly
- {scope constraints}

Notes:
- {content guidance}
- {special requirements}
```
