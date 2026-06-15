# Handoff Guidelines

Use this file to keep the skill's responsibility boundary clear: when it should stay in front, when it should move into implementation, and when it should shift to another skill or mode.

## Role Of This Skill

- Elicit missing quality context through natural follow-up questions
- Fill in missing context before implementation around success criteria, failure handling, compatibility, impact radius, extensibility, and similar concerns
- Deepen the user's thinking without unnecessarily holding onto design review or implementation work

## When To Move Into Implementation

Stop asking extra questions and move into implementation when all of the following are true:

- High-risk concerns have been resolved
- Required success criteria or out-of-scope boundaries are clear enough
- Any remaining unknowns are within the range that is safe to proceed on with assumptions

## When To Shift To A Design-Review Skill Or Mode

Consider shifting when one of these becomes the main issue:

- The task is now about comparing design options
- The task is now about evaluating responsibility split, boundaries, aggregates, or dependency direction
- The task is now about assessing blast radius or long-term change resilience
- The task now needs judgment of design quality, not just elicitation of missing context

If a separate design-review skill is available, prefer using it. If not, shift into normal design-review reasoning rather than treating this as a hard stop: stop foregrounding elicitation and continue with normal design-review reasoning.

## When To Skip And Return To Normal Handling

- Investigation only
- Local refactor
- Typo, wording cleanup, lint, format, or local rename
- Very small changes with almost no quality tradeoff

## When To Stop And Reconfirm

Do not push through with assumptions if any of the following remain unresolved:

- Authorization, access boundaries, or sensitive data handling
- Whether existing APIs or external contracts may break
- Processing that may violate data integrity
- Failure handling where retry, recovery, or compensation changes the implementation structure
