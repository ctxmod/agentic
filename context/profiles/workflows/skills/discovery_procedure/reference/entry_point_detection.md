# Entry Point Detection Procedure

Identify public interfaces that external consumers or runtime environments invoke. Entry points reveal the codebase's intended usage surface, distinguishing public APIs from internal implementation.

## Prerequisites

- Completed repository structure analysis
- Identified primary programming language(s)
- Access to source files via Glob, Grep, and Read tools
- Build configuration files located (package.json, Cargo.toml, go.mod, pyproject.toml)

## Steps

### 1. Language Detection

Confirm primary languages from file extensions and build configuration:

```bash
# TypeScript/JavaScript: package.json, tsconfig.json
# Go: go.mod, .go files
# Rust: Cargo.toml, .rs files
# Python: pyproject.toml, setup.py, __init__.py
# Java/Kotlin: pom.xml, build.gradle.kts
```

### 2. Pattern Application

Search for language-specific entry point indicators using Grep with targeted patterns:

| Language | Entry Point Indicators | Grep Pattern |
|----------|------------------------|--------------|
| TypeScript/JavaScript | `export` declarations, `index.ts` barrels, `main` in package.json, `bin` scripts | `^export\s+`, `^export\s+(default|class|function|const|interface|type)`, `"main"\s*:` |
| Go | `package main`, `func main()`, capitalized identifiers (exported) | `package main`, `^func main\(\)`, `^[A-Z]\w+` |
| Rust | `pub` modifiers, `lib.rs`, `main.rs`, `mod.rs` barrel files | `^pub\s+`, `^fn main\(\)`, `mod\s+\w+;` |
| Python | `if __name__ == "__main__"`, `__all__` lists, `__init__.py` exports | `^__all__\s*=`, `if __name__ == ["']__main__["']` |
| Java/Kotlin | `public class`, `public static void main`, package-info annotations | `public\s+class\s+\w+`, `public\s+static\s+void\s+main` |

### 3. Candidate Verification

Read files matching patterns to confirm entry point status:

- Check if exports are used by other modules (import references)
- Verify main functions are actual executable entry points
- Confirm barrel files re-export public APIs
- Examine CLI configurations for command definitions

### 4. Entry Point Classification

Categorize discovered entry points:

- **Library exports**: Public APIs consumed by other code
- **Executables**: CLI commands, binaries, main functions
- **Event handlers**: API routes, webhooks, message handlers
- **Runtime hooks**: Lifecycle callbacks, initialization functions

### 5. Surface Mapping

Document entry point locations with file:line references. Trace import chains to understand which modules expose public interfaces versus internal implementation.

## Decision Points

### Multiple Entry Point Types

When discovering mixed entry point types (library + CLI), prioritize based on repository type:

- **Library project**: Focus on exported symbols, barrel files
- **Application project**: Focus on main functions, CLI commands
- **Hybrid**: Document both categories separately

### Ambiguous Exports

Not all `export` or `pub` declarations represent intended public APIs. Apply these criteria:

- Exported symbols imported by tests only → internal implementation
- Exports in `internal/` directories → private despite visibility modifiers
- Barrel files re-exporting from subdirectories → public API surface

### Framework Conventions

Some frameworks generate entry points automatically:

- Next.js: API routes in `pages/api/` or `app/api/`
- Spring Boot: `@RestController`, `@Controller` annotations
- Django: views, URL patterns in `urls.py`

Prioritize framework conventions over generic patterns.

## Success Criteria

- All public entry points identified with file:line references
- Entry points classified by type (library, executable, event handler)
- Public API surface distinguished from internal implementation
- Language-specific patterns applied correctly
- Framework conventions recognized and handled

## Common Issues

### False Positives

Overly broad Grep patterns match non-entry-point exports. Refine patterns with anchors (`^export\s+`) and context requirements.

### Test Pollution

Test files expose entry points that don't exist in production code. Exclude `**/*.test.*`, `**/__tests__/**`, `**/test/**` from entry point scans.

### Generated Code

Build artifacts in `dist/`, `build/`, or `target/` create noise. Focus searches on source directories only.

### Monorepo Complexity

Multiple packages require per-package entry point analysis. Parse workspace configuration and analyze each package independently.
