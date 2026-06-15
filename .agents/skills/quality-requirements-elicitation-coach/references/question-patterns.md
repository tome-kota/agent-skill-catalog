# Question Patterns

Use this file to decide which concerns to prioritize for each request type and how to translate them into natural questions. Do not use all of it every time. Read only the parts that matter.

## Table of Contents

- Common rules
- New implementation
- Change to existing behavior
- Bug fix
- Review-feedback follow-up
- Short prompts for light triggers
- When the user does not answer everything

## Common Rules

- First estimate the request type and the weight of the change.
- For heavy triggers, ask 3 to 5 questions. For light triggers, ask 1 to 2 questions by default.
- Go beyond 5 questions only when multiple high-risk concerns are present and known context is not enough to proceed safely.
- Keep one question focused on one concern.
- When possible, include at least one of success criteria, failure behavior, existing impact, future extensibility, or operational/test concerns.
- Avoid using quality-characteristic or subcharacteristic names directly.
- Do not re-ask concerns that are already clear from the conversation.

Priority order for concern selection:

This is the priority order among concerns that are relevant to the current request. For a minor UI-centered change, do not ask about security or compatibility just because they rank higher in the abstract.

1. Security, authorization, auditability
2. Backward compatibility, external contracts, data integrity
3. Failure behavior, recovery, availability
4. Success criteria, out-of-scope boundaries, user impact
5. Future extensibility, maintainability, testability
6. Performance, usability, portability

Default way to choose:

- First choose one concern that would be costly to get wrong.
- Then choose one concern that is likely to cause rework if left implicit.
- For new implementation, success criteria or out-of-scope boundaries are often the next best addition.
- For changes to existing behavior, compatibility or side-effect boundaries are often the next best addition.
- Only add future extensibility, performance, operations, or testing concerns if there is still room.

## New Implementation

Common priorities:

- Success criteria
- Out-of-scope boundaries
- Failure behavior
- Impact on existing users or integrations
- Future rule growth or extensibility
- Performance, security, or operational constraints

Common questions:

- What conditions should count as "working as expected" for this feature?
- Are there cases you explicitly do not want to handle in this iteration?
- If this fails, how should that be surfaced to the user or operator?
- Is it mandatory to avoid impact on existing users or integrations?
- Do you expect more conditions or exception rules like this later?
- Are there constraints around permissions, audit, or response time that I should preserve up front?

## Change To Existing Behavior

Common priorities:

- How much behavior may change
- Backward compatibility
- Impact on surrounding features or operational flow
- Balance between minimal change and future maintainability

Common questions:

- How much of the current behavior must stay unchanged?
- Are there any users, callers, or flows that this change must not impact?
- Should I optimize for the smallest possible fix here, or should I also improve future changeability?
- Is there any related test or validation scenario that absolutely must keep working?

## Bug Fix

Common priorities:

- What counts as fixed
- Depth of recurrence prevention
- Acceptable side effects
- Failure behavior

Common questions:

- What state should count as "fixed"?
- Should I prioritize stopping the symptom quickly, or do you want me to go deeper into root-cause correction?
- How much existing behavior is acceptable to change as part of this fix?
- Is there any additional concern you want guarded against to reduce similar failures in the future?

## Review-Feedback Follow-Up

First classify the comment:

- If it is mainly style, naming, or readability, usually lean toward skipping this skill.
- If it touches safety, responsibility split, exception handling, compatibility, performance, or security, consider triggering this skill.

Common priorities:

- How far to follow the comment
- The quality intent behind the comment
- Impact radius
- Local patch versus structural cleanup

Common questions:

- Is this comment asking only for cleaner presentation, or does it imply a change to behavior or responsibility boundaries?
- Is it acceptable if addressing this comment affects surrounding code or calling patterns?
- Should I make the smallest change that addresses the comment, or should I also move toward a shape that is less likely to regress later?

## Short Prompts For Light Triggers

If returning only one short question, prefer these shapes:

- How much of the current behavior must stay unchanged?
- Is a clear explicit error response enough on failure here?
- Do you expect more similar changes like this later?
- Are there any users or callers that this change must not affect?
- Are there any constraints I should preserve before I implement this?

## When The User Does Not Answer Everything

If the user says "you decide," follow this order:

1. Summarize the unanswered concerns in one line.
2. Decide whether each concern is safe to assume or requires stopping for confirmation.
3. Only for safe concerns, place a conservative or safety-oriented assumption.
4. State that you are proceeding under those assumptions.

Concerns that require stopping and confirmation:

- Authorization, access boundaries, or confidential data handling
- Whether existing APIs or external contracts may break
- Processing that could cause data loss, duplicate execution, or integrity violations
- Failure handling where retry, recovery, or compensation changes the structure or integrity control model

Concerns that are often safe to assume:

- Optimization strength when no explicit performance target is given
- How much to pre-build for future extensibility
- Fine details of UI wording or interaction feel
- Relative priority of logging depth or test depth
- Failure handling limited to returning a simple explicit error for validation or business-rule failures

Examples:

- I will proceed assuming backward compatibility must be preserved and breaking changes should be avoided.
- I will proceed assuming there is no strict performance target, so I will prioritize readability and maintainability over aggressive optimization.
- I will proceed assuming no automatic recovery or retry is required, and that a simple explicit error response plus traceability is enough.
