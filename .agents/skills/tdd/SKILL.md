---
name: tdd
description: A skill for implementing production behavior by using observed failing behavior tests one at a time through public interfaces. Use it for red, green, refactoring, regression tests, bug fixes, and reviewing the TDD chain in AI-generated implementations.
---

# TDD

## 1. Purpose

`/tdd` is an implementation skill for changing production behavior by driving work with behavior tests that can be observed through a public interface or stable boundary. It is not generic testing advice. It is a way of implementing real changes through this sequence: observable behavior -> public interface -> observed failing behavior test -> minimal production code change -> observed green -> refactoring while green.

## 2. What TDD Means In This Skill

In this skill, TDD means:

- Choose one observable production behavior.
- Express it as a behavior test through a public interface or stable boundary.
- Run a focused test and observe it fail as expected.
- Implement only the production code needed to make that test pass.
- Confirm green.
- Refactor only while related tests stay green.

In this skill, TDD does not mean:

- Adding tests after implementation
- Directly testing every internal helper
- Mocking internal collaborators to verify call counts
- Writing all planned behavior tests up front
- Treating any failing test as a valid red
- Replacing an observed red with manual checking

## 3. When To Use It

Use it when:

- Adding or changing production behavior
- Fixing a bug that needs a regression test
- Designing behavior through a public interface or stable boundary
- Reviewing the TDD chain of AI-generated implementations or pull requests

Do not use the normal TDD cycle when:

- Doing pure investigation without changing behavior
- Making documentation-only or configuration-only changes
- Doing only mechanical renames, moves, or formatting

If needed, use exception mode from `references/legacy-and-exceptions.md`.

## 4. Mode Selection

At the start, state which of these modes the request belongs to:

- Implementation mode: Add, change, or fix production behavior. Required evidence is the target behavior, the public interface or stable boundary, the observed red, the minimal green, and the observed green.
- Review mode: Evaluate whether an existing implementation, pull request, plan, or AI-generated code preserves the TDD chain. Required evidence is the target cycle, sequence evidence, observed red or observed green depending on what evidence exists, whether the minimal green is valid, and re-green if refactoring happened. Read `references/review-mode.md`.
- Exception mode: The normal TDD chain cannot be applied directly. Required evidence is the reason for the exception, the constraint, the alternative taken, and the remaining risk. Read `references/legacy-and-exceptions.md`.

If the mode is ambiguous, clarify it before proceeding with implementation. For review requests, do not drift into implementation steps. Prioritize evidence gathering and judgment.

## 5. Core Principle

Every new or changed expected behavior has a corresponding behavior test. Tests verify outcomes a caller or user can observe, not private methods or internal call order. A TDD chain is only valid when it is connected by evidence of an observed red and an observed green.

## 6. TDD State Model

```text
Plan
-> Choose one behavior
-> Write one behavior test
-> Run a focused test
-> Dirty red or clean red
-> Reach clean red
-> Minimal green
-> Confirm green
-> Refactor if needed
-> Confirm green again
-> Move to the next behavior
```

A dirty red is a failure that does not yet prove the missing behavior. Examples: the test file will not load, an import cannot be resolved, a symbol is missing, a function signature does not match, or setup fails before the assertion is reached. During dirty red, only changes that let the test reach its assertion are allowed, such as an empty stub, exporting a missing symbol, adjusting a signature, or returning a neutral placeholder value. Real business logic, generic implementation, future behavior, and unrelated refactoring are not allowed. These temporary changes must be removed once they have served their purpose after reaching clean red. If they remain in the final state, explicitly justify them as current behavior.

A clean red is when the test reaches its assertion for a new or changed expected behavior and fails because the target production behavior does not yet exist. Production logic may be added only after a clean red.

Minimal green is the smallest production code change that makes the current clean red pass. Do not add future options, extra error handling not required by the current behavior, new public methods, abstractions without pressure, or test changes made just to fit the implementation. Do not pass the test with test-only branching, hard-coded values, or dummy logic without real implementation intent. Choose a natural implementation for the same class of input, but do not add untested separate behaviors or branches. Here, a natural implementation means the smallest generalization that works for the same input family as the current failing test without introducing extra branching or a broader public surface. When judgment is unclear, read `references/guarded-cycle.md`.

Refactoring-safe green is the state in which related tests are green and refactoring is allowed. Clarifying renames, duplication removal, helper extraction, type improvement, deepening a module, reducing coupling, and improving test helpers are allowed. New behavior, changes to externally observable behavior, assertion changes that merely ratify the implementation, and refactoring during red are not allowed.

## 7. Standard Implementation Procedure

1. Choose one observable behavior as a small vertical slice.
2. Choose the public interface or stable boundary.
3. Write one behavior test.
4. Run a focused test and observe either dirty red or clean red.
5. If it is dirty red, make only the minimum change needed to reach the assertion.
6. Once clean red is observed, make the minimal production code change for green.
7. Confirm observed green with the focused test and related tests.
8. Refactor only if needed and only while green, then confirm green again.
9. Move to the next behavior.

Do not write many tests first and implement them all later. Do not implement many behaviors first and debug the full test suite afterward.

## 8. Planning Protocol

Before implementation, give a short plan:

```text
Behavior:
Public interface or stable boundary:
Test location:
Expected red:
Minimal green hypothesis:
Risks or unknowns:
```

Do not ask the user for confirmation every time. Ask only when the public interface is unclear, the priority of behaviors is ambiguous, business rules conflict, existing ADRs or design constraints are unclear, or test difficulty implies an interface or design change.

## 9. Cycle Rules

- Each cycle handles exactly one behavior.
- Before implementation, write one behavior test.
- Run a focused test and observe red.
- During dirty red, only stubs, signature adjustments, export fixes, neutral placeholder returns, and minimal wiring are allowed.
- Temporary stubs, placeholder returns, and temporary wiring introduced for dirty red must be removed as soon as they are no longer needed in clean red or minimal green. If they remain, justify them as current behavior.
- Real logic requires a clean red first.
- Minimal green must not go beyond the behavior that is currently failing.
- Refactor only while green.
- After refactoring, rerun related tests.

## 10. Evidence Contract

Evidence during the work can stay concise:

```text
Next behavior:
Target test:
Focused command:
Red:
Failure summary:
Green:
Pass summary:
Regression:
```

At completion, report:

```text
Implemented behavior:
Added test:
Target test:
Focused command:
Observed red:
Failure summary:
Observed green:
Pass summary:
Checks not run:
Alternative checks:
Regression confirmation:
Refactoring:
Mocks:
TDD exceptions:
Untested risks:
```

Always write `observed red` and `observed green`. TDD depends on observed feedback, not claims.
`Target test`, `focused command`, `failure summary`, and `pass summary` are the minimum items needed for a reviewer to replay the chain.
Do not treat checks that could not be run as observed green.

## 11. Stop Conditions

Immediately stop production implementation and move to the recovery protocol if any of these happen:

- New or changed expected behavior entered production code without an observed red
- A test added after implementation is being presented as TDD
- A test claimed as the red for new or changed expected behavior passes immediately
- The failure is still dirty red but real logic is about to be added
- Multiple behaviors are being implemented in one cycle
- The implementation goes beyond the behavior that is currently failing
- Internal collaborators are being mocked without boundary-level justification
- Refactoring begins while related tests are still red

Recovery method:

- Name the broken TDD state
- If needed, isolate or revert code that introduced behavior too early
- Write or repair the behavior test
- Reach clean red
- Implement minimal green
- Ask for user confirmation only if recovery specifically requires it

## 12. Reference Guide

Read `references/behavior-tests.md` when deciding the test boundary, the test name, or whether a test is looking at behavior or implementation detail.

Read `references/guarded-cycle.md` when the red or green state is unclear, when implementation may be too early, or when overimplementation is suspected.

Read `references/interface-design.md` when behavior is hard to test or the public interface looks unstable.

Read `references/mocking-policy.md` before introducing mocks, spies, fake time, fake networks, fake database boundaries, or call-count assertions.

Read `references/legacy-and-exceptions.md` when changing legacy code, doing a spike, doing mechanical refactoring, or using a TDD exception.

Read `references/review-mode.md` when reviewing the TDD discipline of an existing implementation, pull request, plan, or AI-generated code.

## 13. Completion Checklist

- For each cycle, exactly one production behavior was chosen.
- A behavior test was written through a public interface or stable boundary.
- Observed red was recorded, and dirty red was not used as justification for real logic.
- Clean red existed before production logic.
- Minimal green did not go beyond the current behavior.
- Temporary dirty-red stubs, placeholder returns, and temporary wiring do not remain in the final state, or they are justified as current behavior.
- If refactoring happened, it happened only while green.
- Related tests were rerun after refactoring.
- Mocks were avoided, or justified as real boundaries.
- Target test, focused command, failure summary, and pass summary were recorded so the TDD chain can be rechecked.
- Checks that could not be run were not treated as observed green and were recorded as alternative checks or untested risks.
- If the normal TDD chain could not be applied, the exception was recorded.
