# Pattern Detection Procedure

## Overview

Pattern detection identifies recurring structures across file sets through frequency analysis and similarity grouping. The procedure combines quantitative metrics with qualitative similarity assessment to extract architectural signatures.

## Prerequisites

- File corpus assembled from Phase 1 discovery
- Analysis scope defined (specific file types or directories)
- Search tools available (grep, glob, similarity detection)

## Steps

### 1. Frequency-Based Detection

Identify structures occurring across multiple files using occurrence counting.

**Implementation**:
1. Extract candidate patterns: function signatures, class definitions, imports, directory structures
2. Count occurrences across the corpus
3. Filter to patterns appearing in threshold number of files (typically 3+)
4. Rank by frequency and coverage

**Example**:
```bash
# Extract interface definitions
grep -r "interface.*{" src/ | cut -d: -f2 | sort | uniq -c | sort -rn

# Count shared imports
grep -r "from.*import" src/ | awk '{print $2}' | sort | uniq -c | sort -rn
```

### 2. Similarity-Based Grouping

Group patterns that are semantically related but syntactically distinct.

**Implementation**:
1. Cluster functionally equivalent patterns:
   - Heading variations: `## Authentication`, `## User Login`, `## Sign In`
   - Naming alternatives: `handle_*`, `process_*`, `execute_*`
   - Structural equivalents: classes vs closures, interfaces vs type aliases
2. Identify transformation patterns: one pattern refactors to another
3. Map variation families (2-5 variants per pattern)

**Example**:
```
Handler pattern family:
- RequestHandler.handle()
- Processor.process()
- Executor.execute()
```

### 3. Structural Analysis

Examine organization and placement patterns across directories.

**Implementation**:
1. Map directory hierarchies to identify layering conventions
2. Trace file placement rules (e.g., all models in `models/`, utilities in `utils/`)
3. Detect cross-boundary dependencies
4. Identify architectural patterns through placement signatures

**Example signatures**:
- Factories: `src/{module}/factories/{name}.factory.{ext}`
- Adapters: `src/adapters/{target}/{implementation}.{ext}`
- Handlers: `src/handlers/{domain}/{action}.{ext}`

### 4. Dependency and Interface Analysis

Build relationship graphs to uncover interaction patterns.

**Implementation**:
1. Extract import/require statements across modules
2. Identify public APIs (exported symbols) versus internal calls
3. Build dependency chains: which modules depend on which
4. Detect layer boundary violations (e.g., presentation layer importing infrastructure)

**Example**:
```python
# Detect cross-layer imports
grep -r "from.*infrastructure.*import" src/presentation/

# Identify factory classes
grep -r "class.*Factory" src/ | grep -v test
```

## Decision Points

**Threshold selection**: Choose minimum occurrence count based on corpus size.
- Small projects (<50 files): 2+ occurrences
- Medium projects (50-200 files): 3+ occurrences
- Large projects (200+ files): 5+ occurrences

**Similarity cutoff**: Decide when variants represent distinct patterns.
- Different names, identical structure: same pattern
- Same name, different implementations: distinct patterns
- Variations within 30% structural similarity: same pattern family

**Search scope**: Balance breadth versus precision.
- Broad search captures more noise
- Narrow search may miss cross-cutting patterns
- Start broad, then refine to high-confidence matches

## Success Criteria

- **Coverage**: 80% of files examined contain at least one identified pattern
- **Repetition**: Each pattern appears in minimum threshold of files
- **Clarity**: Pattern descriptions use concrete examples from actual codebase
- **Distinctness**: Pattern families have clear differentiation criteria
- **Actionability**: Results inform architecture extraction or refactoring decisions

## Common Issues

**False positives from boilerplate**: Generated code, test fixtures, or framework scaffolding create artificial patterns. Exclude:
- `node_modules/`, `vendor/`, `target/`
- Auto-generated files
- Test directories (unless analyzing test patterns)

**Over-aggressive clustering**: Grouping dissimilar patterns obscures meaningful variation. Maintain separate entries when:
- Implementation differs substantively
- Usage contexts are incompatible
- Refactoring between variants requires substantial work

**Search scope creep**: Analyzing entire codebase yields shallow results. Restrict to:
- Specific domains (e.g., authentication, data access)
- Architectural layers (e.g., controllers, repositories)
- File type subsets (e.g., service classes only)

**Threshold mismatch**: Fixed thresholds don't scale across corpus sizes. Adjust minimums based on:
- Corpus size (larger = higher threshold)
- Directory depth (deeper = higher threshold)
- Module granularity (finer = lower threshold)
