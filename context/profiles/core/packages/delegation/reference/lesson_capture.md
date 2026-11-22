# Delegated Lesson Capture

The repeatable pipeline that turns a friction finding into rendered context rules without the orchestrator hand-stepping each phase. It is commissioned once — a single brief naming the lesson — and then executed end to end by delegated phases, each owning one stage and reporting back. Dispatching the pipeline as a unit is the point: its stages were once assembled ad hoc, worked, and then had to be re-invented from scratch the next time because nothing made them discoverable.

## The five stages

1. **Commission.** The operator or main agent names the lesson in one brief: the evidence behind it, the target rule or reference file it should land in, and the scope it applies to. A commission that omits any of the three sends the authorship phase guessing.
2. **Authorship.** A `write_context_phase` sub-agent drafts the rule or reference from the brief. Authoring doctrine — file structure, generalization, component syntax, line budgets — lives with the `authoring` profile of the writing_context module (`github.com/ctxmod/writing_context/context`); this pipeline points at it and does not restate it.
3. **Nested self-check.** A `validation_objective` sub-agent verifies the draft against the commission before anything renders: marker greps that discriminate the new content, a render check of the rendered output, and line-budget checks. A draft that fails here never reaches the render stage.
4. **Render.** Run `ctxmod vet --root .` and render the module (`ctxmod render`), then verify the new content in the rendered output with a pinned, unwrapped `rg --no-ignore`; a wrapper or shim on `PATH` can mangle or silently drop matches in gitignored rendered trees.
5. **Local excludes.** Record consumer-local divergences where the profile must not generalize: host-specific behavior, single-repo facts, and content a consumer repo already owns stay out of the shared profile and land in the consumer's local layer.

## Why it is codified

The pipeline was assembled once ad hoc, at roughly twenty-one minutes and ~145K output tokens of delegated work, and was the proximate cause of an entire round-1 failure family not recurring in the following rebuild. Its steps — nested validation, byte-identity checks, the raw (unwrapped) scan prescription — were independently re-invented inside that one run, which is the signal that a procedure exists but is not discoverable. The census criterion — the next comparable session dispatches the pipeline in one commission, without per-step steering — is observational and non-gating.

## Related

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/orchestration_overview.md" /> — the corpus entry point; its topic-index row for this file is the sanctioned entry.
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/subagent_mantra.md" /> and `subagent_report_verification` in the ctxmod builtin `delegation` profile — the sub-agent operating contract and the report-verification mechanics the nested self-check leans on.
- `ctx:github.com/ctxmod/agentic/context::shared::harness@shared_tree/reference/harness_traps.md` — why the rendered-output grep is pinned and why filtered output is never evidence.
