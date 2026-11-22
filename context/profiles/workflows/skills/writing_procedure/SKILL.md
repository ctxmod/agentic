---
description: "Execute artifact creation — produce the document at the designated path following the organization, creation, and summary-production procedures for the report or work-item variant."
binds_to_depth: procedure
---

# Writing Procedure

Execute artifact creation at the designated output path. Writing
procedures are the smallest unit of writing work: an agent at this
depth takes a single artifact specification (type, output path,
inputs) and produces the document. Unlike objectives, which determine
WHAT to write and WHY, procedures determine HOW the writing happens.

## Procedural Variant Dispatch (intent decision 5 — conditional links)

Read the reference file matching the writing situation. Do not read
all of them — the flat and `academic/` variants share leaf names
(`creation.md`, `organization.md`) but apply to different situations.
Pick the row whose "When" column matches.

### When each variant applies

- **General (flat) variants** — when the report uses inline
  attribution (`path/to/file.ext:15-42`, `[observation: context]`),
  generic source tables, or the artifact is a project-internal
  deliverable. These are the default; start here unless the artifact
  explicitly follows scholarly conventions.
- **Academic variants** — when the report cites external literature
  with numbered citations (`[1] Author, A. et al. (Year)…`),
  uses paper-summary / concept-note / structure-note templates, or
  follows a literature-review or evidence-extraction methodology.
  Read the `academic/` variant INSTEAD of the flat one when scholarly
  conventions apply; the flat variant's generic guidance does not
  cover citation practice.
- **Summary production** — when the artifact is a cross-report
  synthesis (multiple source reports compressed into a consolidated
  overview). Read alongside the matching report-structure variant
  from 
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary.md" />
.

### Dispatch table

| When | Read |
|------|------|
| General report — determine output path and filename | `reference/organization.md` |
| General report — frontmatter, sections, attribution | `reference/creation.md` |
| Cross-report synthesis (any domain) | `reference/summary-production.md` |
| Academic / scholarly report — output organization (atomic notes, cross-refs) | `reference/academic/organization.md` |
| Academic / scholarly report — citation practice, Sources section | `reference/academic/creation.md` |
| Academic / scholarly report — paper summaries, concept notes, meeting notes | `reference/academic/note-templates.md` |

The `academic/` variants and flat variants are **not additive** —
pick one family based on citation convention. The summary-production
procedure is additive: read it alongside the matching creation
variant when the artifact is a synthesis.

## Core Behavior

1. **Identify variant** — general vs academic (citation convention is
   the discriminator); determine whether the artifact is a synthesis
   (read `summary-production.md` additively).
2. **Resolve path** — read `reference/organization.md` (or
   `reference/academic/organization.md`) for path construction rules.
3. **Author content** — read `reference/creation.md` (or
   `reference/academic/creation.md`) for frontmatter, sections, and
   attribution requirements.
4. **Apply schema** — read the matching structure variant under
   
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_analysis.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_discovery.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_implementation.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_plan.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_remediation.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_research.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_validation.md" />
 for the report
   type's required sections and length guidelines.
5. **Verify** — check completeness against the applicable checklist,
   including the artifact-type budget: run it against the composed
   draft before step 6 writes the file; do not write the file first
   and count later.
6. **Write** — produce the artifact at the reserved report path (or
   {WORK_ITEM_TYPE}_PATH).
7. **Return** — brief summary (2–3 sentences) with output path; do
   not return full content.

## Coordination Pattern

Writing procedures receive a fully-specified artifact request from
the writing objective (or directly from a workflow phase). They do
not decide WHAT to write; they execute the HOW.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />

## Tool Constraints

**Allowed**:
- Read: Input materials, schema references, related artifacts
- Glob: File discovery for verification
- Grep: Content search for verification
- Write: Artifact at designated output path only

**Forbidden**:
- Modifying source materials
- Launching sub-agents
- Writing outside the designated output path
- Deciding artifact type or output path (objective-scope decisions)

## Parent-Layer Dispatch

This procedure is dispatched by the shared `writing_objective`,
which in turn is dispatched by four sibling capability phase skills
in the writing domain:

| Phase skill | Procedures typically dispatched |
|-------------|---------------------------------|
| `write_report_phase` | organization, creation (general or academic), summary-production (synthesis reports) |
| `write_item_phase` | organization, creation (work-item schema) |
| `write_context_phase` | organization, creation (context content schema) |

Intent file creation moved out of the writing domain to the `intent`
domain; the `intent_write` phase skill (at
the `intent_write` skill) absorbs the workflow and dispatches
this procedure for step mechanics.

Ad-hoc procedure-depth agents may also load this skill for general
writing step mechanics outside any of the four capability phase
chains.

- the /write command selection guidance — cross-capability routing for
  artifact creation (reports, work items, context content, intents) —
  <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/write.md" />

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Reading all reference files | Wastes context; flat and academic variants collide on leaf names | Pick the row from the dispatch table whose When column matches |
| Omitting attribution | Unverifiable claims | Every claim gets a source (`path:line`, `[1]` citation, or `[observation: …]`) |
| Returning full content | Bloats orchestrator context | Return 2–3 sentence summary + output path |
| Writing outside the reserved report path | Violates delimitation | Reject the request or escalate |
| Re-deciding artifact type | Procedure-scope overreach | Escalate to objective; procedures execute the HOW |

## Source-Path Note

This skill consolidates `reference/procedures/reports/**` (6 files)
into its `reference/` subtree, shape-preserving. The originals stay
on disk until I1032 (deletion issue). Cross-domain consumers
enumerated in the I1030 report residual log; S116-I1039 rewires them.
