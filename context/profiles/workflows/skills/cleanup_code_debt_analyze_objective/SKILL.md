---
description: "Analyze a single code-debt target (compat shim, reference-preserving duplicate, or DRY/SSOT violation). Enumerate every reference via the symbol index and lexical search, classify the category, emit a removal + reference-update plan. Does not modify code — the acting phase applies findings after the verify-before-remove gate passes."
binds_to_depth: objective
---

# Cleanup Code-Debt — Analyze Objective

Receive a single code-debt target (a file, symbol, or small scope),
read it, enumerate **every** reference via the symbol index and lexical search,
classify the debt category, and emit a structured removal +
reference-update plan the acting phase can apply with minimal
re-reading.

This objective is **read-only**. It does not modify code — removing a
compat shim or duplicate before the phase verifies the reference set
is the exact failure mode the verify-before-remove gate exists to
prevent.

## Core Behavior

1. **Consume**: Read the target descriptor from `INPUT_REPORT` (a
   file path, symbol name, or scope passed by the phase).
2. **Classify**: Determine which of the three categories the target
   belongs to (see Three Categories below). The classification drives
   which verification steps apply.
3. **Enumerate references**: Use the symbol index for definition-level
   references and lexical search for lexical patterns. Both are required
   — neither alone is sufficient (see Tool Usage below).
4. **Propose removal + update plan**: For each reference, specify
   whether it must be updated (and to what) or can be left alone.
5. **Report**: Write structured findings to the reserved report path. The phase
   reads the report; the objective does not return findings inline.

## Three Categories

The three categories are distinct. Verification steps differ per
category — do not collapse them into one generic "debt" bucket.

### 1. Compat Shims

A backward-compatibility or migration layer that has outlived its
migration window. The shim exists so old references keep compiling
while the canonical implementation lives elsewhere.

**Identification signals:**

- Type aliases pointing to a new package or location
  (`type OldConfig = newpackage.Config`).
- Wrapper functions whose body is a single call-through to a newer
  function, with no added behavior.
- `// Deprecated:` markers that have lived past their intended removal
  date (check git history for when the deprecation was added).
- Migration layers documented as temporary in comments or commit
  messages but never removed.
- Re-exports of symbols from a new location
  (`var OldName = newpackage.NewName`).

**Verification steps:**

1. Enumerate every reference to the shim symbol (the alias, wrapper,
   or re-export) via the symbol index `analyze_codebase`.
2. Confirm the canonical implementation is stable — not itself a
   transitional location. A shim pointing to another shim is a chain
   that must be unwound to the true canonical site.
3. For each reference, determine whether updating it to the canonical
   location is mechanical (import swap, type rename) or requires
   judgment (different signature, different package boundary).
4. Check for behavioral differences — a wrapper that adds caching,
   validation, or logging is NOT a pure shim; removing it changes
   behavior.

**Removal plan shape:** `update` (each reference site, with the new
target) + `remove` (the shim file or declaration).

### 2. Reference-Preserving Duplicate Files

A file that exists in two locations, where the duplicate is kept
solely so references (imports, build paths) do not break instead of
updating the references to point at the canonical source.

**Identification signals:**

- Two files with identical or near-identical content under different
  paths (verify via content comparison).
- One copy has a comment like "kept for backward compatibility" or
  "do not remove — X imports this".
- One copy is the clear original (deeper commit history, richer
  comments, colocated tests); the other is the copy.
- The duplicate's imports point outward; the canonical source is
  self-contained or imports from established packages.

**Verification steps:**

1. Identify the canonical source (deeper history, richer content,
   colocated tests). If neither is clearly canonical, halt — the
   phase needs a human decision on which survives.
2. Enumerate every reference to the duplicate path (imports, build
   tags, documentation links) via lexical search `search_codebase`. Go
   imports are the primary surface; also check non-Go references
   (markdown, YAML, shell scripts).
3. Confirm the canonical source provides every symbol the duplicate
   exports. A missing symbol means the duplicate is load-bearing for
   that symbol — flag it rather than proposing wholesale removal.
4. Check for divergence — if the duplicate has drifted from the
   canonical source (local edits not reflected upstream), removal
   would lose those edits. Flag for human reconciliation.

**Removal plan shape:** `update` (each import/reference site, with
the canonical path) + `remove` (the duplicate file).

### 3. DRY / SSOT Violations

Duplicated functions, constants, or maps that should converge on a
single source of truth. Unlike the duplicate-files category, the
duplication is at the symbol level within otherwise-distinct files.

**Identification signals:**

- The same function implemented independently in multiple packages
  (same name, same logic, same signature).
- The same constant defined in multiple places (e.g.
  `const MaxRetries = 3` in two packages).
- The same map or lookup table duplicated with identical keys and
  values.
- Near-identical helpers that differ only in trivial ways (logging
  prefix, error wrapping) that a parameter could absorb.

**Verification steps:**

1. Enumerate every definition of the duplicated symbol across the
   scope via the symbol index `analyze_codebase` (definition lookup) and
   lexical search `search_codebase` (lexical pattern match for
   same-named constants and maps the symbol index may not index as definitions).
2. Determine the canonical location — the package most consumers
   already depend on, or the lowest layer in the package dependency
   graph. If the candidates are in different layers, the lowest layer
   wins (a Layer 0 package cannot import a Layer 1 package).
3. For each definition site, enumerate its call sites. Every call site
   must switch to the canonical definition or be explicitly justified
   to keep its local copy (e.g. avoiding an import cycle).
4. Check for semantic drift — two functions named `ValidateInput` may
   have diverged in what they validate. Byte-identical bodies are
   safe to consolidate; divergent bodies require human judgment on
   which behavior is canonical.

**Removal plan shape:** `keep-canonical` (the chosen definition site)
+ `update` (each call site that switches to the canonical definition)
+ `remove` (each non-canonical definition).

## Tool Usage — symbol index and lexical search

This objective **consumes** a definition-level symbol index and a lexical search. It does not
re-implement reference lookup (no manual `grep` across the tree when
the indexed tools cover the same ground).

### Symbol index — definition-level code intelligence

Definition-level reference lookup (a tree-sitter or language-server backed index; the repository's code-intelligence tool). Use for:

- "Who calls function X?" — returns call sites indexed by tree-sitter.
- "Who imports type Y?" — returns import sites.
- "Where is symbol Z defined?" — resolves to the definition location.

A tree-sitter index typically covers Go, TypeScript/TSX, Rust, and Python. It is the
authoritative surface for definition-reference relationships in those
languages. Prefer it over lexical search when the target is a named
symbol.

### Lexical search — ripgrep, fd, ast-grep

Lexical pattern search (ripgrep + fd + ast-grep, scoped by the
repository's ignore patterns). Use for:

- String literals and comments that mention the symbol by name but
  are not indexable references (e.g. error messages, documentation).
- Non-Go references (markdown links, YAML paths, shell scripts).
- Pattern-shaped searches ("all `const X =` definitions matching this
  shape") where a symbol-level query is too narrow.
- Cross-language duplication (the same constant in a Go file and a
  TypeScript file).

### Why both are required

The symbol index misses lexical patterns (comments, strings, non-indexed file
types). Lexical search misses semantic relationships (it finds the text
`OldConfig` but cannot tell you whether it is a definition, a
reference, or a string literal). For the verify-before-remove gate to
hold, the reference set must be **complete** — run both tools and
merge their results, deduplicating by file:line.

## Report Structure

The analysis report carries one section per target, structured so the
acting phase can apply it without re-reading the source:

```
## <repo-relative path or symbol>

### Classification

- category: compat-shim | reference-preserving-duplicate | dry-ssot-violation
- canonical-target: <path:symbol or path> (the surviving location)
- confidence: high | medium | low
- behavior-preservation-risk: none | low | medium | high

### Reference Enumeration

- tool: the symbol index analyze_codebase
  query: <symbol or path>
  references:
    - path/to/caller.go:42 — update to <canonical-target>
    - path/to/other.go:108 — update to <canonical-target>

- tool: lexical search search_codebase
  query: <pattern>
  references:
    - path/to/README.md:15 — documentation link, update to canonical path
    - path/to/config.yaml:8 — config reference, update to canonical path

### Removal Plan

- action: remove
  target: <path:lines or symbol>
  precondition: every reference above is updated first

- action: update-canonical
  target: <path:symbol>
  note: no change needed — canonical site is already correct

### Verification Summary

- total references enumerated: N
- references requiring update: M
- references requiring no action: N - M
- outstanding (cannot resolve mechanically): K
```

Each finding carries:

- **category** — one of the three categories.
- **canonical-target** — the surviving location after removal.
- **confidence** — how certain the classification is. Low confidence
  routes to human review in the phase.
- **behavior-preservation-risk** — none (pure mechanical removal),
  low (wrapper with no added behavior), medium (slight signature
  difference), high (semantic drift between duplicate and canonical).
  High risk blocks automatic removal.
- **references** — the complete enumerated set, per tool, with the
  update target for each.
- **removal plan** — ordered steps the phase executes.

Target length: 500–1500 words per target. The findings must be
self-contained — the phase reads the report, not the source files.

## Tool Constraints

**Allowed:**

- Read: target source files, reference sites discovered by the symbol
  index / lexical search, prior INPUT_REPORT
- Symbol index: definition-level reference enumeration
- Lexical search (ripgrep, fd, ast-grep): lexical pattern search
- Grep / Glob: targeted lookups when the indexed tools need
  supplementation (non-Go files, build tags)
- Bash: `git log` / `git blame` to date deprecations and identify
  canonical vs. duplicate via commit history
- Write: analysis report at the reserved report path only

**Forbidden:**

- Modifying source files (removals and reference updates are the
  phase's job, not the analysis objective's)
- Launching sub-agents
- Writing outside the reserved report path

## Response Format

After writing the report, return:

```
Wrote code-debt analysis report to <reserved report path>.

Summary: [N] targets analyzed; [C] compat shims, [D] duplicates,
[R] DRY/SSOT violations.
Total references enumerated: [N]. Requiring update: [M].
Notable: [one sentence on the highest-risk finding].
```

Do not return the full findings. The report file is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping lexical search when the symbol index returns results | Lexical references (comments, strings, non-Go files) are missed | Run both tools, merge results |
| Skipping the symbol index when lexical search returns results | Semantic relationships (call vs. definition vs. string) are lost | Run both tools, merge results |
| Proposing removal before enumerating every reference | The verify-before-remove gate cannot hold | Enumerate first, propose second |
| Classifying a wrapper with added behavior as a pure compat shim | Removal changes behavior | Check for caching, validation, logging in the wrapper body |
| Collapsing the three categories into one | Verification steps differ per category | Classify first, then apply category-specific steps |
| Returning findings inline instead of in the report | Phase cannot re-read without re-running analysis | Write to the reserved report path; return summary only |
| Proposing removal when confidence is low | Gate cannot distinguish "safe" from "unchecked" | Route low-confidence findings to human review in the phase |
| Treating a shim-chain (shim → shim → canonical) as a single removal | Intermediate shim breaks if the chain is unwound out of order | Resolve to the true canonical site; flag the chain in the report |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

## Parent-Layer Dispatch

This objective is dispatched by the `cleanup_code_debt_phase` skill,
once per target. The phase passes the target descriptor as
INPUT_REPORT and receives the analysis report path back. The phase
consolidates findings, runs the verify-before-remove gate, and
applies removals.

## Manual Invocation Only

This capability is manual-invocation only (intent §3 D2, §5 scope,
§7 AC item 26). No hook, no CI, no schedule. The objective is
launched only when the operator explicitly invokes
`/cleanup_code_debt`.
