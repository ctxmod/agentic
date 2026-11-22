# Work Item Writing

Use when producing work items that require proper schema structure, parent references, and quality prose. Always uses Opus.

Brief template for a sub-agent (model: opus):

```text
Write {work item type} for {scope description}.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_items.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
- `{parent work item path if applicable}`
- `{discovery report if available}`

Read if relevant:

- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_items_epic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_items_issue.md`
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_items_story.md`
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_items_task.md`

- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_structure_epic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_structure_issue.md`
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_structure_story.md`
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_structure_task.md`

Parameters:
- {WORK_ITEM_TYPE}_PATH={output path}
- PARENT_PATH={parent work item path if applicable}

Delimitations:
- Write work item only; do not create child items
- Follow schema structure exactly
- Reference parent rather than duplicating context

Notes:
- {scope clarifications}
- {specific requirements}
```
