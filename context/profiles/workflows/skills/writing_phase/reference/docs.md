# Documentation Writing

Phase for creating or updating documentation artifacts using documentation procedures

Use when creating or updating documentation artifacts. Uses Opus for prose quality. Select the appropriate documentation procedure based on document type: general procedures for all documentation, technical procedures for code-related documentation, specification procedures for formal specification documents.

Brief template for a sub-agent (model: opus):

```text
Write {documentation type} for {subject}.

Compulsory reads:
- the `writing_objective` skill
- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`
- `reference/procedures/docs/**/*.md`
- `{discovery/analysis report}`

Read if relevant:
- `ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/pruning.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

Parameters:
- OUTPUT_PATH={documentation path}
- DOC_TYPE={general|technical|specification}
- INPUT_REPORT={discovery/analysis report path}

Delimitations:
- Write documentation only; do not implement
- Follow procedure structure from compulsory reads
- Reference principles rather than restating them
- Use general examples unless path-scoped

Notes:
- Documentation procedures live in `reference/procedures/docs/`
- Select procedure based on DOC_TYPE: general/ for all docs, technical/ for code docs, specification/ for formal specs
- Post-writing validation available via validation/docs phase
```

## Related

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::writing_phase/reference/general.md" />
- `reference/orchestration/phases/validation/docs.md`
- `reference/orchestration/phases/research/docs.md`
- `ctx:github.com/ctxmod/writing/context::shared::procedures@documentation/PACKAGE.md`
