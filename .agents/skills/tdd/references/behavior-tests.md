# Behavior Tests

Behavior tests verify production behavior that a caller or user can observe. The goal is not to confirm which private method ran internally. The goal is to confirm that the contract of a public interface or stable boundary is being honored.

## Test Boundaries

Preferred boundaries:

- Public API endpoints
- Entry points such as commands, handlers, CLI operations, or component boundaries
- Domain operations called from application code
- Database boundaries when persistence itself is the behavior

Do not directly test private methods. If some behavior can only be reached through a private helper, first suspect that the public interface may be too large, too hidden, or poorly bounded.

Do not couple tests to internal call order. If a behavior-preserving refactor breaks the test, the test is too dependent on implementation detail.

## Test Intent

Good test intents:

- "Returns a validation error when the submitted email address is invalid"
- "Keeps the existing subscription active when payment authorization is declined"
- "Records an already-imported item only once even if the same source row is processed twice"

Bad test intents:

- "Calls email validation before saving"
- "Calls the user repository lookup exactly once"
- "Sets the private field to pending"

Good examples describe observable behavior. Bad examples mainly lock in implementation details.

## Test Shape

One test should usually verify one logical behavior. If the test name contains "and", it may be packing multiple behaviors together. If those behaviors are separable from the caller's point of view, split them.

Test names should describe behavior, not implementation. Put the condition and the observable outcome in the name.

Assertions should check results that the caller depends on:

- Return values
- Returned responses
- Observable state transitions
- Persisted records when persistence is the behavior
- Messages or events when messages or events are part of the boundary contract

Use direct database inspection carefully. Prefer behavior-visible outcomes unless persistence writes themselves are the production behavior or the database is the stable boundary.

## Refactor-Resistant Tests

If a test breaks after a behavior-preserving refactor, it may be too tightly coupled to internal structure.

Helpful test pressure:

- Clarifies the result the caller needs
- Exposes awkward inputs or outputs
- Finds hidden side effects
- Pushes toward a smaller public surface

Test pressure to avoid:

- Freezing private helper names
- Freezing collaborator call order
- Copying the current implementation
- Reimplementing the production algorithm inside the mock
