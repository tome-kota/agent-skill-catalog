# Interface Design

TDD should apply pressure to the interface, not freeze a bad internal structure in place. When behavior is hard to test, treat that as design feedback.

Difficulty in testing often signals difficulty in use, a poor boundary, or hidden side effects. Before reaching for private-method tests or internal mocks, reconsider the public interface or stable boundary.

## Design Guidance

- If a dependency is a real boundary, prefer receiving it rather than creating it internally.
- Prefer returning results over hiding everything behind side effects.
- Keep the public surface small.
- Use stable boundaries in tests.
- Do not create shallow modules just to make tests easier.
- Let behavior tests expose awkward inputs, awkward outputs, and responsibility-placement problems.

## Stable Boundaries

A stable boundary is a contract a caller can reasonably depend on.

- Request and response shapes
- Command inputs and outputs
- Domain operations exposed to application code
- Event or message schemas
- Persistence contracts when persistence is the behavior

Do not treat an internal function as a stable boundary just because it happens to be easy to call.

## Deep Modules

A good module has a small interface and useful depth. It hides meaningful complexity behind a small interface.

Do not add thin wrappers just for testing. A wrapper that only forwards calls without creating a real boundary can increase brittleness rather than testability.

## Feedback From Testability

When behavior tests are hard to write, check:

- Is the caller-visible behavior clear?
- Is the operation carrying too many responsibilities?
- Are dependency boundaries hidden inside the function?
- Are results observable only through brittle side effects?
- Would a smaller, deeper public interface make the behavior easier to express?

Change the interface only when it improves the production design. Do not change it just to shorten tests.
