# Report Writing

Use when producing reports that synthesize findings from multiple input sources. Uses the default model (no Opus override) because report writing synthesizes from existing content rather than producing original prose.

Brief template for a sub-agent:

```text
Write {report type} report summarizing {scope}.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/reports.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
- `{input reports to synthesize}`

Read if relevant:

- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/reports_summary_decision.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/reports_summary_executive.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/reports_summary_handoff.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/reports_summary_progress.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/reports_summary_technical.md`

- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_summary_decision.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_summary_executive.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_summary_handoff.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_summary_progress.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_summary_technical.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORTS={paths to source reports}

Delimitations:
- Synthesize from provided inputs
- Follow report schema
- {scope constraints}
```

## Variant-Specific Notes

This variant does not use `model="opus"`, unlike the other writing variants. Report writing synthesizes from existing content rather than producing original prose, so the default model provides sufficient capability.
