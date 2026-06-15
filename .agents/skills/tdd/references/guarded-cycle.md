# Guarded Cycle

The guarded cycle is state management for preserving the TDD chain.

```text
Observed red -> clean red -> minimal green -> observed green -> refactor while green
```

Red must be observed. A test that already passes is not evidence that a new test protects a new behavior. A test added afterward is not, by itself, evidence that the missing behavior failed first, so it does not prove a TDD chain.

## Dirty Red

Dirty red is a failure that does not yet prove the missing behavior. Examples include the test file not loading, unresolved imports, missing exports, a mismatched signature, or setup failing before the assertion is reached.

Changes allowed during dirty red:

- Empty stubs
- Missing exports
- Signature adjustments
- Neutral temporary return values
- Minimal wiring needed to reach the assertion
- But no real business logic

Do not add real business logic during dirty red.
Stubs, placeholder returns, and temporary wiring added for dirty red should be removed once they are no longer needed after reaching clean red. If they remain in the final state, explicitly justify them as current behavior.

## Clean Red

Clean red means the test reaches its assertion and fails because the production behavior needed for the new or changed expectation is missing. Production logic may be added only after clean red.

If a test claimed as red passes immediately, stop. The behavior may already exist, the test may not be asserting the intended behavior, or the wrong test may be running.

## Minimal Green

Minimal green is production code that exists only to make the clean red for the current behavior pass. It is not permission to complete the entire design. You may not pass the test with test-only branching, hard-coded values, or dummy logic without real implementation intent.
By natural implementation, this skill means the smallest generalization that works for the same input family as the current failing test without increasing branching or expanding the public surface.

Examples of overimplementation:

- Adding options the current behavior does not require
- Adding error handling not present in the observed failing behavior test
- Introducing abstractions with no duplication or change pressure
- Adding public methods for future behavior
- Changing multiple behavior paths in one cycle
- Weakening assertions to make the test pass
- Adding branches or constants that only satisfy the test data

## Rationalization Detection

Stop and inspect the TDD state when these phrases appear:

- "We'll add the test later"
- "This doesn't really need a test"
- "Manual checking is enough"
- "The implementation is already done"
- "This is just wiring"
- "This is only a small change"
- "Testing this is too difficult, so let's skip it"
- "The mock proves it works"
- "All the tests pass, so the new behavior is probably covered too"
- "It will be easier to do TDD after refactoring first"

Recovery is not punishment. It is a practical reset. Name the broken state, isolate or revert behavior that came too early if needed, write or fix the behavior test, reach clean red, then implement minimal green.

## Recovery Protocol

1. Name the state: dirty red, missing red, overimplementation, post-hoc test, or refactoring during red.
2. Identify the smallest problematic production behavior.
3. If production behavior came too early, isolate or revert the too-early part as needed.
4. Write or repair the behavior test through a public interface or stable boundary.
5. Run a focused test and observe either dirty red or clean red.
6. If it is dirty red, add only empty stubs, missing exports, signature adjustments, neutral placeholder returns, or minimal wiring until the assertion-level failure is reached.
7. After clean red, add minimal green.
8. Confirm observed green.
9. Refactor only while green, then confirm green again.
