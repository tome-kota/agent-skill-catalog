# Test Integrity Review

Purpose: detect test weakness, missing coverage, and verification dishonesty.

Review only tests, coverage signals, verification quality, fixtures, snapshots, mocks, and CI trust.

Flag:

- tests deleted without justification
- assertions weakened
- snapshot updates without semantic explanation
- skipped tests
- relaxed CI conditions
- reduced coverage thresholds
- tests that only mirror implementation
- happy-path-only tests for risky logic
- missing boundary tests
- missing error-path tests
- missing authorization or security tests when relevant
- failure conditions hidden by mocks

Severity calibration:

- Blocking: deleted or weakened tests, skipped CI, reduced thresholds, or missing tests for authorization, data integrity, invariant, error handling, migration, persistence, or public-boundary behavior when the changed behavior is newly introduced, security/data-critical, or previously covered.
- Warning: missing meaningful tests for changed branch logic, boundary conditions, compatibility behavior, or non-trivial user-visible behavior.
- Note: small maintainability gaps in tests where existing coverage still exercises the changed behavior.

Do not flag:

- tests removed because behavior was intentionally removed
- snapshot updates with clear semantic explanation
- refactoring-only changes where existing tests fully cover behavior

Return findings using [../constructs/finding-format.md](../constructs/finding-format.md).
