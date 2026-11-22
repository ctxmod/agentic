# Workflow Reference Discovery

Discover reports, work items, docs, and compulsory reads per sub-agent.

Use before launching workflow sub-agents to identify phase-specific reference files. The main agent invokes this discovery to build targeted compulsory reads for each sub-agent launch. This ensures each sub-agent launched during a workflow receives only the context it needs, and all context that is relevant to it specifically, improving efficiency and reducing context bloat.

Discover the specific reports, work item files, documentation, and compulsory-read reference files relevant to each sub-agent in a workflow. The main agent provides general phase files and workflow files; this agent responds with targeted context per sub-agent type so the main agent can construct focused Task launches.

Additionally, when an intent file exists for the current session, the preflight agent identifies files especially relevant to that intent and provides augmentations.

The main agent typically references only general phase files (e.g., `skills/discovery/shared/reference/_index.md`). This agent examines variant phase files (e.g., `skills/discovery/shared/reference/scan.md`, `skills/discovery/shared/reference/architecture-synthesis.md`) and related phase categories (e.g., `research/*.md` variants relate to discovery phases) to recommend which `subagent_type` values and variants best fit each phase—without returning the specialized phase files themselves. The main agent receives better orchestration context this way.

Use this preflight phase to:

1. Invoke this agent with general phase files and workflow files referenced by the current workflow
2. Get a structured map of relevant files per sub-agent type
3. Use the map to build targeted compulsory reads for each Task launch
4. Apply recommended subagent_types when variant phases fit better than the general template

Usage:
- Provide general phase file paths as `{PHASE_FILES}`
- Provide workflow file paths as `{WORKFLOW_FILES}`
- Provide task description and context as `{TASK_CONTEXT}`
- Provide specific work items that will be worked on as `{WORK_ITEMS?}`, if known
- Provide applicable intent as `{INTENT_FILE?}`, if known
- Provide current date/time (take it from the host, e.g. `date -u`)
- Use output to construct compulsory reads for each sub-agent launch
- Only include files mapped for that specific agent type and variant

Brief template for a sub-agent:

```text
Discover reports, work items, documentation, and compulsory-read references for each sub-agent in the workflow.

Compulsory reads:
- `{PHASE_FILES}`
- `{WORKFLOW_FILES}`
- `{TASK_CONTEXT}`
- `{WORK_ITEMS?}`

Delimitations:
- Discovery only; respond directly without writing reports
- Return concise reference map organized by phase and sub-agent type
- Look for reports within the last 2 days only (consider current {date} and {time})

Notes:

Report discovery:
- Search for recent reports that inform the current task
- Include only informational reports: execution, analysis, discovery, research reports
- Exclude process reports: validation reports, remediation reports, verification reports, correction plans, execution plans
- Report paths follow the pattern {REPORTS_BASE}/{date}/{time}-{slug}.md

Work item discovery:
- Preferred: `ctxmod work list [--parent <parent-id>]` for inventory and statuses. Fall back to file search when the work store is unavailable.
- `work_store_first` in the ctxmod builtin `work_discipline` profile
- Find related work item files (ISSUE.md, EPIC.md, STORY.md, TASK.md) and note their completion status
- Include work items that provide context for the current workflow objective

Documentation discovery:
- Find documentation files relevant to sub-agent objectives
- Exclude legacy and deprecated documentation

Reference file discovery (compulsory reads per sub-agent):
- Identify relevant objectives, procedures, principles, and report schemas for each sub-agent type
- Do NOT return phase files or workflow files as compulsory reads
- Return files that sub-agents should read: objectives (reference/objectives/), procedures (reference/procedures/), principles (reference/principles/), schemas (skills/writing/objective/reference/structure/)

Variant and subagent_type recommendations:
- Parse all agent-template blocks in the provided general phase files
- Examine variant phase files in subdirectories of each referenced phase (e.g., discovery/*.md, research/*.md, analysis/*.md)
- Also examine related phase categories (research variants relate to discovery; analysis variants relate to planning)
- Recommend preferred subagent_type values and variants for each phase instead of returning the specialized phase file paths
- The main agent uses these recommendations to select the right agent type per launch

When INTENT_PATH parameter provided:
1. Read intent file to understand developer requirements
2. Discover related files:
   - Documentation explaining concepts mentioned in intent
   - Principle files related to constraints specified
   - Procedure files for workflows mentioned
   - Code files implementing similar patterns
   - Prior reports on related topics
   - Work items with related context
3. For each file: Single sentence explaining relevance to intent
4. Check for reference errors: Identify incorrect paths, outdated references
5. Return augmentations: Structured list of relevant files with relevance explanations

Output format (respond directly):
```md
## Intent Augmentations (when applicable)

Intent file: {INTENT_PATH}

Relevant files:
- `{path}`: {single-sentence relevance explanation}
- `{path}`: {single-sentence relevance explanation}

Reference corrections (if found):
- Fix: {incorrect} -> {correct}
- Update: {outdated_reference} -> {current_reference}

Main agent appends these to intent file before launching sub-agents.

## Reference Map

### Phase: \{phase-name}

#### `subagent_type: \{type}` (variant: \{variant-name if applicable})

Compulsory reads:
- reference/objectives/\{...}/\{specific}.md
- reference/procedures/\{...}/\{specific}.md
- reference/principles/\{...}/\{specific}.md
- skills/writing/objective/reference/structure/\{type}.md
- ...

Reports:
- \{REPORTS_BASE}/{date}/{time}-\{slug}.md (execution/analysis/discovery/research/novel info only)
- ...

Work items:
- \{path}/ISSUE.md (status: \{status})
- \{path}/STORY.md (status: \{status})
- ...

Documentation:
- \{path}/\{file}.md

[Repeat for each sub-agent type across all phases]

## Variant Recommendations

### Phase: \{phase-name}

Recommended subagent_type: \{preferred-type}
Reason: \{why this variant fits better than the general template}

[Include only when a specialized variant is preferable]
```

## Discovery Categories

The agent discovers five categories of files, each with specific inclusion criteria.

### Reports

Search for recent reports (within 2-day window) that inform the current workflow. Include only informational reports that contain findings, results, or analysis:

| Include | Exclude |
|---------|---------|
| Execution reports | Validation reports |
| Analysis reports | Remediation reports |
| Discovery reports | Verification reports |
| Research reports | Correction plans |

### Work Items

Find work item files related to the current workflow context. Return file paths and their completion status:

- `EPIC.md`, `STORY.md`, `ISSUE.md`, `TASK.md`
- Note status from the record (`ctxmod work show`: open, in_progress, blocked, done, cancelled)
- Include items that provide context for sub-agent objectives

### Documentation

Find documentation files relevant to sub-agent objectives. Exclude files in `legacy/`, `deprecated/` directories or files marked as deprecated.

### Reference Files (Compulsory Reads)

Identify files that each sub-agent should receive as compulsory reads. These are the reference files each agent needs to execute well—not the phase files that describe orchestration.

| Include | Exclude |
|---------|---------|
| Objectives (`reference/objectives/`) | Phase files (`reference/orchestration/phases/`) |
| Procedures (`reference/procedures/`) | Workflow files (`reference/orchestration/workflows/`) |
| Principles (`reference/principles/`) | Other orchestration files |
| Report schemas (`skills/writing/objective/reference/structure/`) | |

## Variant Recommendation Process

The main agent references general phase files. This agent examines the variant subdirectories and related phase categories to recommend better-fit agent types:

1. **Parse general phase files**: Extract agent-template blocks from provided phase files
2. **Examine variant files**: Read variant phase files in subdirectories (e.g., `discovery/scan.md`, `analysis/gap.md`)
3. **Cross-reference related categories**: Research variants relate to discovery; analysis variants relate to planning; remediation variants relate to validation
4. **Recommend subagent_types**: For each phase, recommend the variant that best fits the current task—return the recommended `subagent_type` and rationale, not the variant file path
5. **Preserve main agent abstraction**: The main agent does not need to know about specialized phase files; it needs to know which agent type to launch
```

## Related

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
- the `discovery_objective` skill
