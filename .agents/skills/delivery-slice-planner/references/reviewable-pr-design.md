# Reviewable PR Design

## Purpose

Design a PR sequence that lets reviewers follow intent and risk.

The purpose of PR splitting is not only to make diffs small. Each PR should make clear what reviewers are being asked to judge, while avoiding excessive mixing of mechanical changes, structural changes, behavior changes, migration, and release work.

## Types Of Changes To Separate

- `Mechanical changes`: Renaming, formatting, import cleanup, type-name changes, simple moves.
- `Structural changes`: Responsibility separation, abstraction, module moves, dependency direction changes.
- `Behavior changes`: Changes to specifications, branches, validation, authorization, or state transitions.
- `Contract changes`: Changes to APIs, DTOs, events, DB schemas, or external interfaces.
- `Migration changes`: Data migration, compatibility handling, dual old/new support, cleanup.
- `Verification changes`: Test additions, test cleanup, monitoring, logs, confirmation procedures.
- `UI changes`: Screens, copy, user flows, display conditions.

## Basic Principles

- Do not mix mechanical changes and behavior changes in the same PR.
- When possible, ship structural changes first while preserving behavior.
- For contract changes, write compatibility and user impact in the PR description.
- For migration PRs, state the old state, new state, intermediate state, and rollback path.
- Treat UI changes as rule changes when display conditions or operability conditions change.
- Even for test-only PRs, confirm that the specification intent has not changed.

## Common PR Sequence Patterns

### Structure-First Pattern

1. Add tests or characterization tests that protect existing behavior.
2. Separate responsibilities without changing behavior.
3. Add the new behavior.
4. Remove unnecessary old paths.

### Compatibility-First Pattern

1. Create an entry point that supports both old and new behavior.
2. Add new writes or calls.
3. Migrate readers or consumers in stages.
4. Stop and delete the old path.

### Risk-First Pattern

1. Use a minimal verification PR to check external integration, performance, authorization, migration, or similar risk.
2. Firm up the structure based on the verification result.
3. Add the behavior change.
4. Add release preparation and observability.

### UI-Later Pattern

1. Prepare APIs, state, authorization, and data first.
2. Integrate the UI behind a feature flag or hidden state.
3. Enable the user flow.
4. Confirm monitoring and support investigation paths.

## Bad PR Splitting

- PR 1 is a huge "preparation" PR and nobody can tell what should be reviewed.
- PR 1 introduces abstraction, PR 2 changes behavior, and PR 3 is assumed to fix bugs.
- Data migration, API changes, UI enablement, and old-path deletion are in the same PR.
- Tests are grouped into the final PR.
- The only reviewer instruction is "please review everything."

## Per-PR Description Template

```md
## Purpose

## Included Changes

## Excluded Changes

## What To Review

## Verification

## Done Condition

## Release And Compatibility Notes
```

## Review Questions

- What is this PR asking reviewers to judge?
- Can reviewers follow specification changes separately from structural changes?
- Can this PR be tested or manually checked on its own?
- Is this a state that may exist in production until the next PR arrives?
- Are there any issues with rollback or coexistence with old paths?
