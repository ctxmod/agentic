# Query Formulation Procedure

Constructing effective search queries requires specificity, technical precision, and context. Well-formed queries return relevant results quickly; poorly formed queries waste research time on irrelevant information.

## Prerequisites

- Clear research objective identified
- Domain terminology familiar
- Technology stack and versions known

## Steps

### 1. Core Component Identification

Identify the primary subject of research:
- **Technology**: Specific language, framework, or tool
- **Concept**: Pattern, technique, or architectural principle
- **Problem**: Error, bottleneck, or challenge

Example: "TypeScript async error handling"

### 2. Context Specification

Add situational constraints:
- **Use case**: Application context (web, CLI, library)
- **Scale**: Load characteristics, data volume
- **Constraints**: Performance, security, compatibility

Example: "TypeScript async error handling serverless functions"

### 3. Technical Precision

Replace generic terms with specific terminology:
- "performance issues" → "memory leaks" or "latency spikes"
- "best practices" → specific patterns or anti-patterns
- "help" → specific problem description

Example: "TypeScript async error handling serverless functions ETag caching"

### 4. Temporal Constraints

Add year filters for fast-moving topics:
- Recent developments: "2024" or "2026"
- Stable topics: omit temporal constraint
- Historical context: specify decade

Example: "TypeScript async error handling serverless functions 2026"

### 5. Desired Outcomes

Specify what the query should return:
- "code examples" or "implementation patterns"
- "benchmark data" or "performance comparison"
- "case studies" or "production experiences"

Example: "TypeScript async error handling Result type implementation examples 2026"

## Decision Points

### Query Length

**8-15 words optimal**. Shorter queries lack context; longer queries over-constrain results.

| Too Short | Too Long | Optimal |
|-----------|----------|---------|
| "PostgreSQL pooling" | "PostgreSQL connection pooling Node.js high load serverless AWS Lambda production monitoring health checks 2026" | "PostgreSQL connection pooling Node.js high load best practices 2026" |

### Specificity Level

Adjust specificity based on research stage:

**Exploration**: Broader queries to understand landscape
- "React state management patterns 2026"

**Implementation**: Narrow queries for specific solutions
- "Zustand middleware persistence React 18 TypeScript"

**Debugging**: Precise queries targeting known issues
- "PostgreSQL connection exhaustion PgPool Node.js 2026"

### Multi-Factor Queries

Combine multiple constraints when relevant:
- **Technology + Pattern + Context**: "Rust async error handling tokio production"
- **Problem + Solution + Language**: "memory leaks Go channels goroutine leaks detection"
- **Framework + Specific Feature + Version**: "Next.js 15 server actions form validation TypeScript"

## Success Criteria

- **First page relevance**: 70% of top 10 results directly address the research question
- **Technical depth**: Results include code examples, benchmarks, or production experiences
- **Recency**: For fast-moving topics, results from last 12-18 months appear prominently
- **Source quality**: Results include official docs, GitHub discussions, academic papers, or technical blogs

## Common Issues

### Over-Specification

Query too narrow returns zero or irrelevant results:
- *Bad*: "TypeScript async await error handling Result type either pattern validation Zod 2026"
- *Good*: "TypeScript async error handling Result type Zod validation 2026"

### Under-Specification

Query too broad returns overwhelming irrelevant results:
- *Bad*: "React performance"
- *Good*: "React 18 useMemo optimization expensive computations large component trees"

### Ambiguous Terminology

Generic terms return off-topic results:
- *Bad*: "Go channels buffering" (could mean data buffers or channel capacity)
- *Good*: "Go channels buffer capacity deadlock patterns"

### Missing Context

Query lacks use case or constraint:
- *Bad*: "PostgreSQL indexing"
- *Good*: "PostgreSQL partial indexes multi-tenant tenant_id query performance"

## Query Templates

### Pattern Research
```
[Technology] + [Pattern Name] + [Context/Use Case] + [Optional: Year]
```
Example: "Rust trait bounds generic data structures lifetime constraints"

### Problem Solving
```
[Technology] + [Error/Symptom] + [Context] + [Solution Keywords]
```
Example: "Node.js memory leaks EventListener cleanup development vs production"

### Comparison Research
```
[Technology A] vs [Technology B] + [Specific Dimension] + [Context]
```
Example: "Prisma vs Drizzle ORM TypeScript edge computing serverless"

### Implementation Guidance
```
[Technology] + [Feature] + [Implementation Pattern] + [Code Examples]
```
Example: "FastAPI dependency testing pytest fixtures override patterns"

## Related

- Reference: <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research_procedure/reference/source_evaluation.md" />
