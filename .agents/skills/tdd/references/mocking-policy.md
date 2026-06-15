# Mocking Policy

Mocks are mainly allowed at system boundaries. Use them to observe production behavior without directly touching systems that are unavailable, slow, random, or external.

## Usually Acceptable Mock Boundaries

- External HTTP APIs
- Clocks and time
- Randomness
- File systems
- Networks
- Payment, email, and third-party providers
- Database boundaries when preparing a real database would be disproportionately heavy

Prefer fakes or contract-level test doubles that express the behavior of the boundary clearly, rather than call-count assertions.

## Usually Not Allowed

- Private methods
- Internal modules
- Domain services owned inside the codebase
- Repositories mocked only to assert call counts
- Mocks that reproduce implementation logic

Mocking internal collaborators easily turns behavior tests into implementation-detail tests. If a test only proves that one internal object called another internal object, it probably is not protecting observable behavior.

## Danger Signals

- Mock setup is longer than the assertion
- The test mainly checks "called with these arguments" or call counts
- The test passes even if real behavior is broken
- The test is really testing the mock's behavior
- Removing the mock makes the test meaningless

## Boundary Justification

Before introducing a mock, write a short justification:

```text
Boundary:
Why using the real dependency is disproportionate:
Observable behavior being protected:
Risk if the mock drifts:
How drift will be detected:
```

If the mock is not tied to observable behavior, do not add it.
For `How drift will be detected`, state whether drift is caught by a contract test, a thin integration test, a smoke test, or why none applies.
