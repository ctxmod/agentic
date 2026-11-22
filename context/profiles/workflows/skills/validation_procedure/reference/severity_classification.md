# Severity Classification Procedure

Assign severity levels to validation findings to prioritize remediation. This procedure provides consistent categorization across validation types.

## Prerequisites

- Validation findings documented with descriptions and locations
- Impact assessment context available (artifact purpose, audience, downstream dependencies)
- Classification criteria established or inferred from artifact criticality

## Steps

1. **Establish severity levels** with clear definitions:

   | Level | Definition | Remediation |
   |-------|------------|-------------|
   | **Error** | Finding blocks intended use or propagates harm | Must fix before proceeding |
   | **Warning** | Finding degrades quality but does not block use | Should fix when practical |
   | **Info** | Finding notes improvement opportunity or minor deviation | Fix at discretion |

2. **Assess each finding** against impact dimensions:
   - **Functional impact**: Does finding prevent artifact from serving its purpose?
   - **Downstream impact**: Will finding cause problems in dependent artifacts or processes?
   - **Audience impact**: Does finding mislead, confuse, or harm consumers?
   - **Maintenance impact**: Does finding create technical debt or complicate future changes?

3. **Apply classification rules** based on impact assessment:

   **Error** when any apply:
   - Artifact cannot fulfill primary purpose
   - Finding will propagate incorrect information downstream
   - Finding violates mandatory requirements
   - Finding creates security, safety, or compliance risk

   **Warning** when any apply:
   - Quality significantly degraded but function preserved
   - Finding violates recommended but not mandatory guidelines
   - Finding creates maintenance burden
   - Audience experience negatively affected

   **Info** when:
   - Finding represents style preference or minor improvement
   - Impact limited to aesthetics or consistency
   - Finding does not affect function, downstream, or audience

4. **Consider context modifiers** that adjust severity:
   - Critical artifacts (security, contracts): Elevate warnings to errors
   - Draft or internal artifacts: May demote errors to warnings
   - Automated processes downstream: Elevate formatting issues
   - Human-only consumption: May demote formatting issues

5. **Document classification** with severity, rationale, and any context modifiers applied.

## Decision Points

**Borderline cases**: When finding falls between levels, consider downstream impact as tiebreaker. Higher downstream impact elevates severity.

**Aggregate severity**: Multiple warnings in same area may collectively warrant error treatment. Assess cumulative impact.

**Disputed classifications**: When stakeholders disagree, document the disagreement and default to higher severity until resolved.

**Temporary exceptions**: Some findings may be acknowledged but deferred. Document exception with rationale and expiration.

## Success Criteria

- Every finding assigned severity level
- Classification rationale documented
- Context modifiers noted when applied
- Error findings clearly distinguished for immediate attention
- Aggregate patterns assessed

## Common Issues

**Severity inflation**: Treating all findings as errors reduces signal value. Reserve errors for genuine blockers.

**Severity deflation**: Treating genuine problems as info to avoid remediation work. Assess impact honestly.

**Context blindness**: Applying uniform severity without considering artifact purpose. Adjust for criticality.

**Missing thresholds**: Vague guidelines lead to inconsistent classification. Establish concrete criteria.

**Remediation disconnect**: Severity assigned without considering fix effort. Classification reflects impact, not effort—effort informs prioritization separately.
