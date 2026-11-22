# Import Extraction Procedure

Extract import and dependency statements from source files using language-specific parsing patterns. This procedure produces raw import data for graph construction and dependency analysis.

## Prerequisites

- Source directory exists and is readable
- Language detected (file extension or manifest presence)
- Grep tool available for pattern matching
- Output path identified for import data

## Steps

### 1. Language Detection

Identify the primary language using file extensions and project manifests:

| Indicator | Language |
|-----------|----------|
| `package.json` + `.ts` files | TypeScript/JavaScript |
| `pyproject.toml` or `requirements.txt` | Python |
| `go.mod` | Go |
| `Cargo.toml` | Rust |
| `pom.xml` or `build.gradle` | Java/Kotlin |
| `.csproj` | C# |

Mixed-language codebases require language-specific pattern sets applied to respective file extensions.

### 2. Pattern Selection

Apply import patterns matching the detected language. Use regex-based Grep searches optimized for false-positive reduction.

#### TypeScript/JavaScript

**Value imports**:
```bash
grep -rn "^import \|^const.*=.*require(" --include="*.ts" --include="*.js"
```

**Type-only imports**:
```bash
grep -rn "^import type" --include="*.ts"
```

**Dynamic imports**:
```bash
grep -rn "import(" --include="*.ts" --include="*.js"
```

#### Python

**All imports**:
```bash
grep -rn "^import \|^from " --include="*.py"
```

**Relative imports** (track separately):
```bash
grep -rn "^from \.\." --include="*.py"
```

#### Go

**All imports**:
```bash
grep -rn "^import \"" --include="*.go"
```

**Vendor paths** (flag for internal analysis):
```bash
grep -rn "^import \".*vendor/" --include="*.go"
```

#### Rust

**Use statements**:
```bash
grep -rn "^use " --include="*.rs"
```

**Extern crates**:
```bash
grep -rn "^extern crate" --include="*.rs"
```

#### Java

**Imports**:
```bash
grep -rn "^import " --include="*.java"
```

**Static imports** (flag separately):
```bash
grep -rn "^import static" --include="*.java"
```

#### Kotlin

**Imports**:
```bash
grep -rn "^import " --include="*.kt"
```

#### C#

**Using statements**:
```bash
grep -rn "^using " --include="*.cs"
```

**Static usings**:
```bash
grep -rn "^using static " --include="*.cs"
```

### 3. Parsing and Normalization

Extract structured data from Grep matches:

**Fields to capture**:
- Source file path
- Line number
- Import type (value, type, dynamic)
- Target module/package
- Is external (package registry) vs internal (same repo)

**Normalization rules**:
- Resolve relative paths (`./utils.ts` → full path)
- Strip alias declarations (`import { x as y }` → record as `x`)
- Distinguish side-effect imports (`import './styles.css'`)
- Tag test/mocking imports for separate analysis

### 4. Categorization

Classify imports by dependency layer:

| Category | Detection Rule | Example |
|----------|----------------|---------|
| Internal | Path exists in repo | `import './utils'` |
| External | Node modules / vendor | `import 'lodash'` |
| Stdlib | Language standard library | `import 'fs'` (Node) |
| Type-only | Type imports only | `import type { User }` |
| Dynamic | Runtime import() | `const mod = await import('./plugin')` |

### 5. Output Structure

Write import data to the discovery report:

```markdown
## Import Extraction Summary

- Files analyzed: [count]
- Total imports: [count]
- Internal: [count]
- External: [count]
- Type-only: [count]
- Dynamic: [count]

### Import Statements by File

[Table with columns: file, line, import_type, target, category]
```

## Decision Points

### Multi-Language Projects

When a codebase contains multiple languages:

1. **Partition by extension**: Run language-specific patterns in parallel
2. **Cross-language boundaries**: Flag FFI or interop imports (e.g., Python importing C extensions)
3. **Unified graph**: Track language as edge metadata in the dependency graph

### Monorepo Handling

For monorepo structures:

1. **Detect workspace roots**: Look for `package.json` workspaces, `go.work`, or similar
2. **Tag workspace imports**: Mark imports from other workspace packages as internal-but-cross-package
3. **Preserve package boundaries**: Do not flatten intra-workspace imports

### Build Artifacts

Exclude generated files from import extraction:

- Build directories (`dist/`, `build/`, `target/`)
- Generated code (`*.generated.ts`, `pb.go` protobuf files)
- Vendor directories (unless analyzing vendoring strategy)

## Success Criteria

- **Coverage**: All source files in target directory analyzed
- **Precision**: Zero false-positive imports (verified by manual spot-check)
- **Resolution**: All relative imports resolved to absolute paths
- **Categorization**: Every import tagged with correct category
- **Traceability**: Each import includes source file and line number

## Common Issues

### Minified or Bundled Code

**Problem**: Import statements obscured by bundlers

**Solution**:
- Exclude build artifacts via glob patterns
- Analyze source files only
- Flag presence of bundles as separate finding

### Conditional Imports

**Problem**: Imports inside conditional blocks or dynamic strings

**Examples**:
```typescript
if (process.env.NODE_ENV === 'test') {
  import('./test-setup')
}
const module = require(dynamicPackageName)
```

**Solution**:
- Flag dynamic/conditional imports separately
- Record as "runtime-resolved" dependency
- Note that static analysis cannot determine target

### Macro-Based Imports

**Problem**: Languages with macro systems generate imports at compile time

**Examples**: Rust `macro_rules!`, Scala macros

**Solution**:
- Parse only explicit `use` / `import` statements
- Flag files with heavy macro usage for manual review
- Note limitation in report

### Circular Import Detection

**Problem**: Early detection of circular dependencies during extraction

**Solution**:
- Build adjacency list during extraction
- Run simple DFS for cycles after each module processed
- Flag immediately, defer full analysis to graph construction phase
