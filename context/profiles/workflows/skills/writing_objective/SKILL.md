---
description: "Create structured artifacts following defined schemas — reports, work items, context content (reference/rule/skill/agent), intent, docs."
binds_to_depth: objective
---

# Writing Objective

Create structured artifacts that follow defined schemas and content
guidelines. Writing objectives transform input materials into
well-formed documents at designated output paths. The output type
determines structure; content guidelines ensure clarity and
consistency.

## Artifact Category Dispatch (conditional links)

Read the reference file matching the artifact category. Do not read
all of them — each category is independently applicable and reading
extras wastes context.

| Artifact category | Read |
|-------------------|------|
| Reports (discovery, analysis, validation, summary) | `reports.md` (index), then the matching variant `reports_<variant>.md` — the `reporting` package of this profile |
| Summary reports (decision, executive, handoff, progress, technical) | `reports_summary.md` (index), then the matching variant `reports_summary_<variant>.md` — the `reporting` package |
| Work items (epic, story, issue, task) | `work_items.md` (index), then the matching variant `work_items_<variant>.md` — the `items` package of the agentic `work` profile |
| Agents (ctxmod `agents:` declarations) | `agents_authoring.md` — the `authoring` profile of the writing_context module (`github.com/ctxmod/writing_context/context`) |
| Commands (`commands/<name>/COMMAND.md`) | `commands_authoring.md` — writing_context `authoring` profile |
| Reference files (`reference/*.md`) | `reference_authoring.md` — writing_context `authoring` profile |
| Rules (`rules/*.md`) | `rules_authoring.md` — writing_context `authoring` profile |
| Skills (`skills/<name>/SKILL.md`) | `skills_authoring.md` — writing_context `authoring` profile |

### When each applies

- **Reports**: when the artifact synthesizes findings from prior
  discovery/analysis/validation phases into a structured document at
  a report path reserved just-in-time (`ctxmod work new report
  "<title>"`). Read <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports.md" /> first; then read the
  matching variant (`reports_discovery.md`, `reports_analysis.md`,
  `reports_validation.md`, or `reports_summary.md`) based on report
  type.
- **Summary reports**: when synthesizing across multiple prior reports.
  Read <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary.md" /> first; then the matching variant
  (`reports_summary_{decision,executive,handoff,progress,technical}.md`).
- **Work items**: when creating tracked work units (epic, story,
  issue, task). Read `work_items.md` in the agentic `work` profile's
  `items` package first; then the matching variant
  `work_items_<type>.md`.
- **Context content** (agents, commands, reference, rules, skills):
  read the matching `<category>_authoring.md` in the writing_context
  `authoring` profile for schema and process.

Variant references are additive — read them when even somewhat
relevant; do not read all of them by default.

- the /write command selection guidance — cross-capability routing for
  artifact creation (reports, work items, context content, intents) —
  <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/write.md" />

## Core Behavior

1. **Understand**: Clarify the artifact's purpose, audience, and scope
2. **Gather**: Collect input materials and reference documents
3. **Structure**: Apply the appropriate schema for the artifact type
4. **Author**: Write content following style guidelines
5. **Verify**: Check completeness against schema requirements
6. **Write**: Produce artifact at the designated output path
7. **Summarize**: Return brief summary (2-3 sentences) with output path

## Relationship to Discovery and Analysis

Writing follows discovery and analysis when artifacts require groundwork:

| Phase | Focus | Output |
|-------|-------|--------|
| Discovery | What exists | Source materials, inventories |
| Analysis | What it means | Insights, recommendations |
| **Writing** | **Documenting it** | **Structured artifacts** |

Writing objectives receive inputs from prior phases. They transform
raw findings or recommendations into consumable documents.

## Pre-Writing

Before authoring:

1. Identify artifact type and applicable schema
2. Gather input materials (reports, requirements, source documents)
3. Understand audience and purpose
4. Confirm output path parameter is available
5. Review reference exemplars for the artifact type

## During Writing

Apply these guidelines consistently:

`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`

General writing principles:

- **Precision**: Use exact terminology; avoid vague qualifiers
- **Conciseness**: Eliminate filler; every sentence adds value
- **Structure**: Follow schema; maintain consistent hierarchy
- **Attribution**: Reference sources for all claims
- **Audience**: Match depth and terminology to intended readers

## Schema Adherence

Each artifact type has structural expectations:

- **Required sections**: Must be present with appropriate content
- **Optional sections**: Include when relevant
- **Section ordering**: Follow defined sequence
- **Heading levels**: Maintain proper hierarchy
- **Frontmatter**: Include required metadata fields

Variant objectives define specific schemas for their artifact types.

## Document Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Determine the artifact output path and create the structured document following the report organization and creation procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Writing objectives produce documents at designated output paths. Report
paths are reserved just-in-time by the writing sub-agent
(``ctxmod work new report "<title>"``); work-item paths arrive as
parameters (`EPIC_PATH`, `STORY_PATH`, etc.).

## Tool Constraints

**Allowed**:
- Read: Input materials, reference exemplars, related artifacts
- Glob: File discovery for context gathering
- Grep: Content search for verification
- Write: Artifact at designated output path only

**Forbidden**:
- Modifying source materials
- Launching sub-agents
- Writing outside the designated output path
- Making implementation changes

## Response Format

After writing the artifact, return a brief response:

```
Wrote {artifact type} to {OUTPUT_PATH}.

Summary: [2-3 sentence summary of artifact scope and key content]
```

Do not return the full artifact content. The document is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Writing without schema | Inconsistent structure | Apply artifact-appropriate template |
| Skipping audience consideration | Mismatched depth | Tailor to intended readers |
| Claims without attribution | Unverifiable content | Reference sources |
| Inline quotation | Bloats artifact | Use references with paths |
| Missing required sections | Incomplete artifact | Check schema completeness |
| Returning full content | Bloats orchestrator context | Return summary + path |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile

## Sub-Layer Launch

When this objective requires step-level execution, launch the
`writing_procedure` agent. Procedure agents carry the per-procedure
mechanics under 
- `ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
- `ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/summary_production.md`
.

Brief template for the `writing_procedure` agent — Execute <procedure-name> within writing objective:

```text
<ctxmod_activate skill="writing_procedure" />
Procedure: <procedure-name>
<procedure-specific instructions>
Write to {OUTPUT_PATH} or a report path reserved just-in-time (`ctxmod work new report "<title>"`).
```

Available procedure agents in the writing domain: dispatch against
the procedure variants in 
- `ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
- `ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/summary_production.md`

(organization, creation, summary-production, academic/organization,
academic/creation, academic/note-templates).

## Parent-Layer Dispatch

This objective is dispatched by four sibling capability phase skills
in the writing domain, one per artifact category:

| Phase skill | Artifact categories dispatched | Reference material |
|-------------|--------------------------------|--------------------|
| `write_report_phase` | Reports (discovery, analysis, validation, summary) | the `write_report_phase` skill |
| `write_item_phase` | Work items (epic, story, issue, task) | the `write_item_phase` skill |
| `write_context_phase` | context content (reference, rule, skill, agent, command) | the `write_context_phase` skill (`github.com/ctxmod/writing_context/context`, `authoring` profile) |

Intent files moved out of the writing domain to the `intent` domain.
The `intent_write` phase skill (at the `intent_write` skill)
now absorbs the intent-writing workflow and dispatches this
objective for composition sub-steps; it is launched either
standalone via `/write-intent` or end-to-end through the
`capture_intent` orchestrator skill.

Ad-hoc objective-depth agents may also load this skill for general
writing work outside any of the four capability phase chains. The
shared methodology layer (`writing_phase`) carries the variant-
selection reference (model choice, coordination pattern, transition
criteria, anti-patterns) consumed by this objective and by the
capability phase skills.
