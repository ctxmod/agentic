# Equivalence Verification

Use when validating transformations that must preserve behavior (refactoring, migration). Compares baseline state to transformed state, verifying structural changes and reference integrity while confirming behavior equivalence.

Brief template for a sub-agent:

```text
Verify structural equivalence after transformation.

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `{baseline-structure report path}`
- `{transformation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-verification-structure" --owner <work item, if any>` immediately before writing; returns the path
- ARTIFACT_PATHS={transformed artifacts}

Delimitations:
- Verify structure matches plan expectation
- Do not validate behavior (separate concern)

Notes:
- Compare new structure to plan
- Verify all planned changes applied
- Check for unplanned changes
```

Brief template for a sub-agent:

```text
Verify reference integrity after transformation.

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `{baseline-consumers report path}`
- `{transformation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-verification-references" --owner <work item, if any>` immediately before writing; returns the path
- ARTIFACT_PATHS={transformed artifacts and consumers}

Delimitations:
- Verify all references resolve
- Check for orphaned or broken references

Notes:
- Validate every consumer reference updates
- Check for dangling references to old locations
- Verify no new broken references introduced
```

Brief template for a sub-agent:

```text
Verify behavioral equivalence after transformation.

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `{transformation report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-verification-behavior" --owner <work item, if any>` immediately before writing; returns the path
- SCOPE={transformation scope}

Delimitations:
- Run applicable verification (tests, type checks, etc.)
- Compare behavior before and after

Notes:
- Execute existing verification suites
- Note any behavioral differences
- Distinguish expected vs unexpected changes
```
