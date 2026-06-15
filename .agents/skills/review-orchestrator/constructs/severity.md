# Severity

## Blocking

Use when there is a credible, concrete path to correctness, security, authorization, data integrity, invariant, or test-trust failure.

Require at least one:

- reproducible with realistic input, permission, state transition, or failure mode
- crosses a public API, schema, persistence, tenant, privilege, or trust boundary
- weakens a guard, invariant, authorization check, or test that previously protected risky behavior
- ships uncertain behavior by hiding invalid states, failures, or missing requirements at a public, trust, or data-integrity boundary

Examples:

- invalid state hidden by fallback
- authorization bypass
- aggregate invariant moved outside owner
- test weakened to make change pass
- unsafe type widening across a public boundary

## Warning

Use when the change increases maintainability, coupling, review, or future-change risk but is not immediately unsafe.

Examples:

- unnecessary service abstraction
- duplicated helper
- unclear responsibility split
- compatibility creep with limited scope

## Note

Use for minor issues that are safe to defer.

Examples:

- naming inconsistency
- comment cleanup
- small local readability issue
