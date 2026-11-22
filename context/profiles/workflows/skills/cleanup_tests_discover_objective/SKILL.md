---
description: "Enumerate test files via the repository file index's tag scoping and partition them into ≤30-file groups for downstream per-group analysis."
binds_to_depth: objective
---

# Cleanup Tests — Discover Objective

Enumerate the repository's test files and partition them into
bounded groups so each downstream analysis agent stays under the
≤30-file context ceiling (intent §5). Produces a structured
discovery report listing groups with their member files; emits no
judgments about test quality.

## Core Behavior

1. **Scope**: Resolve the sweep scope from the `scope` argument
   (path or package). Default to the repository root when no scope
   is given.
2. **Enumerate**: Use the repository file index's tag-based scoping
   to list test files. Do not walk the filesystem directly — the index is the
   SSOT for file classification and skip-set semantics.
3. **Partition**: Group files into ≤30-file chunks keyed by package
   or directory so each group's analysis agent reads a coherent
   blast radius.
4. **Report**: Write the partitioned file list to the reserved report path.

## Enumerate via the repository file index

The repository's file index — a role-tagging index over the tree that
records each file's role and extra tags, with the repository's skip set
applied — is the source of truth; its role-tag model decides. Where no
such index exists, fall back to `git ls-files` filtered by each
language's test-file conventions and say so in the report. Test files carry
the `tests` role tag; the test-type tags (`unit`, `integration`,
`e2e`, `benchmark`) refine the corpus.

```
<index> source                    # scope directory list (NDJSON)
<index> files source --dir <dir>  # per-file records with role/extra_tags
```

Select `role == "tests"` from the per-file records (jq or
equivalent). To include context-example tests that read like test
scaffolding, also select `role == "context-examples"`. Prefer the
`tests` role for the canonical sweep; only broaden when the caller
asks for example-style tests.

Scope narrowing — when the `scope` argument targets a path or
package, filter the index result set against that prefix before
partitioning.

## Partition Rule

The ≤30-file bound is a **hard limit, not a guideline** (intent §5).
A single agent reading more produces shallow judgments because
context budget is split across too many files.

Partition strategy, in priority order:

1. **By package** — keep files in the same Go / TS / Rust package
   together so the analysis agent sees a coherent unit. Group size
   target: 15–30 files.
2. **By directory** — when a package exceeds 30 files, split along
   sub-directory boundaries. When a package is small, merge sibling
   packages under the same parent directory until the group reaches
   15–30 files.
3. **By language** — never mix languages within a group. Different
   test frameworks have different redundancy and flakiness signals;
   a mixed group muddies the analysis.

Group IDs are sequential: `group-01`, `group-02`, … so downstream
phases can reference them by ordinal.

## Report Structure

The discovery report carries:

- **Scope** — the resolved scope (path or `repo-root`).
- **Corpus summary** — total file count, language breakdown,
  test-type-tag distribution.
- **Groups** — one section per group, listing:
  - Group ID (`group-NN`)
  - Member paths (repo-relative)
  - Language tag
  - Test-type tags present
  - File count (must be ≤30)

Target length: 300–800 words. The report is a manifest, not an
analysis — no quality judgments appear here.

## Tool Constraints

**Allowed**:
- Bash: read invocations of the repository file index (`source`, `files`, `file`, `modules`)
- Read: cross-check individual files when tag resolution is ambiguous
- Write: report file at the reserved report path only

**Forbidden**:
- Modifying source files
- Launching sub-agents (this objective produces a manifest only)
- Reading full test file bodies — enumeration is tag/metadata only
- Emitting quality judgments (redundancy, flakiness, etc.) — those
  belong to the analyze objective

## Response Format

After writing the report, return:

```
Wrote test-file discovery report to <reserved report path>.

Summary: [N] test files partitioned into [M] groups of ≤30.
```

Do not return the full manifest. The report file is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Walking the filesystem directly | Bypasses the index's skip set and tag model | Use the index's per-file record filtering |
| Groups larger than 30 files | Breaks the analyze objective's context budget | Split along sub-directory boundaries |
| Mixing languages in one group | Framework-specific signals get muddied | Keep one language per group |
| Reading file bodies during enumeration | Wastes context budget on the discovery step | Defer content reading to the analyze objective |
| Quality judgments in the manifest | Confuses downstream consumers | Emit only the partitioned file list |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile

## Parent-Layer Dispatch

This objective is dispatched by the `cleanup_tests_phase` skill as
the first step of the tests sweep. The phase passes the discovery
report path to each per-group `cleanup_tests_analyze_objective`
invocation.

## Manual Invocation Only

This capability is manual-invocation only (intent §3 D2, §5 scope).
No hook, no CI, no schedule. The operator runs `/cleanup_tests`
explicitly when test debt is suspected.
