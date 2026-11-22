# Structural Validation Procedure

Verify that artifacts conform to their defined format and structure requirements. This procedure applies to any artifact with structural expectations: documents, configurations, data files, or reports.

## Prerequisites

- Target artifact identified and accessible
- Structure definition available (schema, template, specification, or reference exemplar)
- Validation scope defined (full artifact or specific sections)

## Steps

1. **Identify structural requirements** from the governing definition. Requirements fall into categories:
   - **Required elements**: Sections, fields, or components that must be present
   - **Permitted elements**: Optional components allowed but not required
   - **Prohibited elements**: Components that must not appear
   - **Ordering constraints**: Sequence requirements for elements
   - **Nesting rules**: Parent-child relationships between elements

2. **Extract artifact structure** by parsing the target. Create an inventory of elements present, their locations, and their relationships.

3. **Check required elements** against inventory. For each requirement:
   - Present and correctly positioned: Pass
   - Present but mispositioned: Flag as ordering violation
   - Missing: Flag as missing required element

4. **Check prohibited elements** against inventory. Any match indicates structural violation.

5. **Verify nesting and ordering** when constraints exist. Walk the structure hierarchy and compare against defined rules.

6. **Assess element completeness** for required elements. An element present but empty or malformed fails structural validation even when technically present.

7. **Document findings** with specific locations and violation types. Group by severity (missing required vs mispositioned vs malformed).

8. **Cite actual hashes for byte-identity claims.** When a finding asserts that a file is byte-identical to a baseline, unchanged, or "not edited" (e.g. a no-marker-edit claim), the evidence MUST cite both the actual working-tree blob hash and the HEAD-blob hash for the specific path — never a bare "matches baseline" or a hardcoded hash lifted from a prior report:
   - **Working-tree blob hash:** `git hash-object <path>` (the current working-tree content).
   - **HEAD-blob hash:** `git rev-parse HEAD:<path>` (the committed content at HEAD).
   - Equality of the two proves the working tree matches HEAD; citing either one verbatim (copied from the command output, not transcribed from memory or a stale report) is the evidence. A bare "matches baseline" or a hash asserted without the command that produced it is insufficient and is a finding of its own (evidence-not-cited).

## Decision Points

**No explicit schema available**: Use reference exemplars to infer structural expectations. Document the inferred requirements before validating.

**Partial structure definitions**: Validate only documented requirements. Note undocumented sections as out-of-scope.

**Conflicting requirements**: Prioritize more specific requirements over general ones. Flag the conflict for resolution.

**Dynamic structures**: When structure varies by type or context, identify the applicable variant before validation.

## Success Criteria

- All required elements present and correctly positioned
- No prohibited elements detected
- Nesting and ordering constraints satisfied
- Elements contain valid content (not empty or malformed)
- Byte-identity / no-marker-edit / unchanged claims cite both the working-tree
  blob hash and the HEAD-blob hash (with the producing command), not a bare
  "matches baseline"
- Findings documented with locations and violation types

## Common Issues

**Schema drift**: Definition and actual practice diverge over time. Validate against the authoritative definition, not common patterns.

**Implicit requirements**: Unstated expectations cause validation gaps. Document discovered requirements for future validation.

**Partial compliance**: Artifact passes some structural checks but fails others. Report all failures rather than stopping at first violation.

**False positives from whitespace**: Element detection may be sensitive to formatting. Normalize whitespace before structure extraction.

**Nested validation scope**: When artifacts contain embedded artifacts, determine whether to validate recursively or treat embedded content as opaque.

**Stale-baseline hash citation (the R-MIN-11 hazard)**: A report may cite a hash that was accurate in a prior working-tree state but is no longer current (e.g. a WIP change landed after the baseline was recorded). A hash cited without the `git hash-object` / `git rev-parse` command that produced it cannot be re-verified and may be accidentally true today but false under any future working-tree-with-WIP scenario. Require the command alongside the hash so the evidence is reproducible.
