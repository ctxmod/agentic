# Write Command Selection Guidance

Selection guidance for the `/write` command — the unified entry
point for artifact creation across work items, context content,
reports, and intent files. This document captures the
cross-capability routing logic, variant-selection tables, and
"which command when" guidance. Execution lives in the phase skills
(`write_item_phase`, `write_context_phase`, `write_report_phase`,
`intent_write`).

## Capability Routing

`/write [artifact-type] [name] [description]` routes to one of four
capability phase skills based on the artifact-type argument.

| Input | Routes To | Capability Phase |
|-------|-----------|------------------|
| `/write epic ...` | `/write-epic` | `write_item_phase` |
| `/write story ...` | `/write-story` | `write_item_phase` |
| `/write issue ...` | `/write-issue` | `write_item_phase` |
| `/write task ...` | `/write-task` | `write_item_phase` |
| `/write reference ...` | `/write-reference` | `write_context_phase` |
| `/write rule ...` | `/write-rule` | `write_context_phase` |
| `/write skill ...` | `/write-skills` | `write_context_phase` |
| `/write agent ...` | `/write-agent` | `write_context_phase` |
| `/write command ...` | `/write-command` | `write_context_phase` |
| `/write docs ...` | `/write-docs` | `write_context_phase` |
| `/write report ...` | `/write-report` | `write_report_phase` |
| `/write intent ...` | `/write-intent` | `intent_write` |

## Work Item Context by Artifact Type

| Artifact Type | Work Item Context | Rationale |
|---------------|-------------------|-----------|
| epic | None (creates context) | Epics are top-level containers |
| story | Optional parent epic | Stories may be standalone or epic-linked |
| issue | Optional parent story | Issues may be standalone or story-linked |
| task | Required parent issue | Tasks always belong to issues |
| reference | Optional | Reports nest in work item if provided |
| rule | Optional | Reports nest in work item if provided |
| skill | Optional | Reports nest in work item if provided |
| agent | Optional | Reports nest in work item if provided |
| report | Optional | Reports nest in work item if provided |
| intent | Optional | Intent nests in work item if provided |

## Variant Comparison

| Command | When to Use |
|---------|-------------|
| `/write` | General entry point, unsure of exact variant |
| `/write-epic` | Creating strategic initiative |
| `/write-story` | Creating feature specification |
| `/write-issue` | Creating work unit description |
| `/write-task` | Creating granular activity |
| `/write-reference` | Creating comprehensive documentation |
| `/write-rule` | Creating behavioral constraint |
| `/write-skills` | Creating capability aggregation |
| `/write-agent` | Creating sub-agent template |
| `/write-command` | Creating command file with workflow design |
| `/write-report` | Creating synthesis document |
| `/write-intent` | Creating intent file |

## Capability-Specific Variations

| Capability | Discovery Focus | Research | Writing Model | Key Validation |
|-----------|-----------------|----------|---------------|----------------|
| Work items | Parent context, siblings | Scope-dependent | Opus | Schema compliance |
| Context Content | Existing content, related files | Best practices (reference) | Opus | Density, cross-refs |
| Report | Source materials | Never | Opus | Schema, sources |
| Intent | Ambiguities, related items | Multi-source | Opus | Structure, content, consistency |

## Common Characteristics

All write commands share these characteristics:

- **Opus for writing**: Writing phases use Opus for quality prose
- **Schema compliance**: Outputs follow defined structures
- **Style standards**: Content adheres to anti-patterns and clarity guidelines
- **Conditional validation**: Validation applies only to schema-validated artifacts
- **Validation loops**: Maximum 3 remediation iterations (when validation applies)
- **Report persistence**: Phase reports enable progress tracking

## Notes

For frequent use, prefer the specialized commands which provide more
focused context and skip routing overhead. When artifact type is
ambiguous from user input, the main agent should clarify before
proceeding. Misrouted artifacts require recreation rather than
conversion.

## Execution Pointers

- `/write-epic|story|issue|task` → launch `write_item_phase` agent
- `/write-reference|rule|skill|agent|command|docs` → launch `write_context_phase` agent
- `/write-report` → launch `write_report_phase` agent
- `/write-intent` → launch `intent_write` agent (or `capture_intent`
  orchestrator for end-to-end capture)

Each capability phase agent has its skill preloaded and links the
shared `writing_phase` methodology layer for variant selection, model
choice, and transition criteria. See
writing_phase (the `writing_phase` skill) and
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`.
