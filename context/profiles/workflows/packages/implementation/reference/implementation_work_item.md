# Work Item Creation

Use when creating work items from planning output. Creates story, issues, and tasks following planned organization with proper parent-child relationships.

Brief template for a sub-agent:

```text
Create {work-item-type} from planning output.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_items_{work-item-type}.md`
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_structure_{work-item-type}.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`
- `{planning report path}`

Parameters:
- {TYPE}_PATH={work_item_path}
- Report: sub-agent reserves just-in-time via `ctxmod work new report "creation" --owner <work item, if any>` immediately before writing; returns the path
- PARENT_ID={parent_id}

Delimitations:
- Create work items only; do not execute

Notes:
- Set parent field appropriately (story references epic, issue references story)
- Include planning context in work item
- Use identifier provided by main agent
- Include research findings if applicable
```
