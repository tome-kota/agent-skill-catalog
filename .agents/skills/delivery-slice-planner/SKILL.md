---
name: delivery-slice-planner
description: Use this skill to convert basic designs, feature designs, technical directions, larger implementation ideas, or AI-generated implementation plans into delivery slices, reviewable PR sequences, test strategies, deployment order, and feature enablement order that can be executed safely in Scrum development. Especially use it when avoiding oversized PRs, deciding implementation order, organizing topics to discuss with a tech lead, separating behavior changes, refactoring, migration, and release work, or shipping incrementally while preserving backward compatibility and rollback options. Do not use it for simple implementation, pure design review, local refactoring diagnosis, ordinary code review, or estimation-only discussions.
---

# Delivery Slice Planning

Convert designs and larger change ideas into ordered work structures that a team can safely implement, review, verify, and release.

The purpose of this skill is not to "split tasks into smaller pieces." It is to preserve the intent of the change while cutting it into units where risk appears early, humans can review the work, tests can verify the intended behavior, and the team can roll back when needed.

## Core Stance

- Preserve the design intent while converting it into implementable units.
- Do not hide dangerous uncertainty until the end.
- Do not pack multiple kinds of decisions into one PR.
- Do not treat tests as an afterthought; map them to the assurance each slice needs.
- Do not assume a single release after everything is complete; reason about intermediate states, production rollout, rollback, and observability.
- Do not hand decisions off to the tech lead without preparation; organize options, risks, recommendations, and the judgments that need confirmation.

## When To Use

Use this skill for:

- Creating delivery slices from basic designs or design notes.
- Turning a large change into a sequence of reviewable PRs.
- Deciding how to separate refactoring, behavior changes, data migration, and UI changes.
- Planning implementation order for API or data changes while preserving backward compatibility.
- Planning work that involves feature flags, staged rollout, rollback, and monitoring checks.
- Organizing topics to confirm with a tech lead before implementation.
- Reworking an AI-generated implementation plan into something realistic for Scrum delivery.

Do not use this skill for:

- Small bug fixes or straightforward implementation.
- Pure design review.
- Pure refactoring diagnosis.
- Code review.
- Validating the product requirements themselves.
- Estimation-only discussions.

For out-of-scope requests, do not force the output into this skill's format. If requirements, design, responsibility boundaries, refactoring direction, or business decisions are too unsettled to form a delivery plan, briefly extract only the decisions or questions that must be settled first.

## Workflow

1. Check the inputs and existing constraints.
2. Frame the delivery target.
3. Separate delivery slices, PRs, deployments, and feature enablement.
4. Choose a slicing strategy.
5. Create delivery slices and a PR sequence.
6. Map review focus and test focus to each slice.
7. Check release order, compatibility, rollback, and observability.
8. Separate tech-lead confirmation points from unresolved issues.
9. Integrate everything into one executable delivery plan.

## 1. Check Inputs And Existing Constraints

First decide whether the user's design notes are sufficient. If relevant information is available, read the minimum needed scope before producing a plan.

- Basic design, detailed design, ADRs, tickets, user stories, acceptance criteria
- Existing code, existing tests, existing PRs, related configuration and jobs
- API specs, DB schemas, events, DTOs, external interfaces, authorization rules
- Release procedures, feature-flag operations, rollback procedures, monitoring, logs, support procedures
- Constraints already agreed with the tech lead or team

Do not start with exhaustive research. To identify the impact surface, prioritize related contracts, callers, existing tests, and release procedures. If full research is needed, continue with a provisional plan and leave it in `Unresolved Issues`.

If information is unavailable, say so. When planning without reading existing constraints, mark plan confidence as `High / Medium / Low` and leave the needed checks in `Unresolved Issues`.

Confidence guide:

- `High`: Key contracts, existing impact, tests, and release constraints have been checked within the minimum needed scope.
- `Medium`: Important assumptions remain, but the overall plan and high-risk areas can be judged.
- `Low`: The plan is based mainly on design notes or requests, with existing impact, contracts, and release constraints mostly unchecked.

## 2. Frame The Delivery Target

Briefly organize:

- The value or business meaning to achieve
- Impact on existing users, existing APIs, existing data, and existing operations
- What would hurt most if the change failed
- Kinds of changes involved: behavior change, structural change, data migration, contract change, UI change, operational change
- Whether the change must finish within one sprint or can be released in stages
- Areas likely to require tech-lead judgment

Do not stop immediately when information is missing. State safe assumptions explicitly. Only raise confirmation items for things that are dangerous to decide unilaterally, such as compatibility, data integrity, authorization, or changes that cannot be safely rolled back.

## 3. Separate Delivery Slices, PRs, Deployments, And Feature Enablement

Do not mix these units:

- `Delivery slice`: A meaningful unit for the team to implement and verify.
- `PR`: A review unit where reviewers can understand intent and diff.
- `Deployment`: A unit that applies code or configuration to an environment.
- `Feature enablement`: A unit that creates user impact through a feature flag, configuration, authorization, exposed entry point, or similar mechanism.
- `Production operation`: A work unit managed separately from PRs, such as backfills, manual execution, operations approval, execution records, and monitoring checks.

One delivery slice may become multiple PRs, and multiple PRs may be deployed together. Code may be deployed while the feature remains disabled. Backfills and migrations must be treated not only as PRs, but also as production operation procedures, execution approvals, execution records, and monitoring checks. For high-risk changes, make these relationships explicit in the plan.

## 4. Choose A Slicing Strategy

Choose one primary strategy first. Do not spread the plan across too many strategies in parallel. Safety-related strategies may be listed as `supporting strategies`.

- `Value vertical slicing`: Slice by units that can validate user value or business value, even if small.
- `Risk-first slicing`: Expose uncertainty, external integrations, performance, authorization, data integrity, and similar risks early.
- `Compatibility layer first`: Build dual support or translation layers first so existing users are not broken.
- `Migration first`: Firm up schema, data, contract, or event migration feasibility early.
- `Structure first`: Add only the minimum responsibility separation or test scaffolding needed before behavior changes.
- `Observability first`: Add logs, metrics, monitoring, or investigation paths before relying on the change's safety.

When strategy choice is unclear, prefer the one that exposes the risk that would be most expensive if discovered late.

If external integrations, authorization, performance, data integrity, migration, backward compatibility, or multi-team coordination are involved, read [references/risk-first-slicing.md](references/risk-first-slicing.md).

## 5. Create Delivery Slices And A PR Sequence

Each slice must include at least:

- `Purpose`: What this slice advances.
- `Included changes`: What belongs in this PR or task.
- `Excluded changes`: What is intentionally kept out.
- `Dependencies`: Earlier slices required and handoffs to later slices.
- `Review focus`: What reviewers should judge.
- `Test focus`: What this slice assures.
- `Done condition`: What makes this slice complete. Include whether main, the integration branch, or the release target remains deployable, and whether the result can safely remain in production until follow-up work happens.
- `Release notes`: Notes about production rollout, feature flags, compatibility, rollback, and observability.

Being "small" is not enough. Each slice must be reviewable, verifiable, and leave a meaningful state for the next piece of work.

If PR splitting, oversized PR avoidance, separating refactoring from behavior changes, contract changes, or migration changes are involved, read [references/reviewable-pr-design.md](references/reviewable-pr-design.md).

## 6. Map Tests And Verification

For each slice, state which assurance is carried at which layer.

- Unit tests: Cheaply assure rules, branches, transformations, and state transitions.
- Integration tests: Assure persistence, APIs, external integrations, and major boundaries.
- E2E tests: Assure representative user paths and regression risks from the user perspective.
- Manual checks: Handle UI, operational procedures, complex exceptions, and first-release checks.
- Production observability: Handle logs, metrics, alerts, and diagnosability for support.

Do not push everything into E2E tests. Choose the cheapest reliable layer that can provide the needed assurance.

## 7. Check Release Safety

Check whether intermediate states may exist in production.

- Can old code and new code coexist?
- Can old data and new data coexist?
- Will old API consumers, new API consumers, batches, admin screens, and external integrations keep working?
- Are feature flags, configuration values, or staged rollout needed?
- During rollback, will data, contracts, or user operation history break?
- After release, can success and abnormal states be observed?

If DB schemas, data migration, API contracts, external integrations, authorization, batches, UI enablement, feature flags, configuration switches, staged rollout, or rollback are involved, read [references/release-safe-sequencing.md](references/release-safe-sequencing.md).

When reading reference files, do not mechanically copy all their content into the output. Read the relevant references and adopt only the viewpoints that affect this plan.

## 8. Separate Escalation Targets

Separate these in the output:

- `Implementation decisions safe to proceed with`
- `Work-splitting decisions to align with the team`
- `Design decisions to confirm with the tech lead`
- `Business or release decisions to confirm with the PO, customer, or operations`

Do not end tech-lead confirmations as bare questions. When possible, include options, risks, a recommendation, and a decision deadline.

## Output Format

Normally use this format:

```md
# Delivery Plan

## Delivery Target

- Plan confidence:

## Slicing Strategy

- Primary strategy:
- Supporting strategies:

## Proposed Slices

| Slice | Deliverable/PR | Deploy/Enablement | Purpose | Included changes | Excluded changes | Dependencies | Review focus | Verification | Done condition | Release notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

## Review Plan

## Test Plan By Slice

## Release And Rollback Notes

## Tech Lead Confirmation Points

## Unresolved Issues
```

For smaller discussions, compress the output to only `Delivery Target`, `Slicing Strategy`, `Proposed Slices`, and `Tech Lead Confirmation Points`. Do not omit `Dependencies`, `Review focus`, or `Done condition`.

For lightweight output, a table is not required. Write each slice as roughly four bullet lines including `what to do`, `dependencies`, `review focus`, and `verification/done condition`.

If a concrete output example is needed, or if you need to confirm the expected output quality for this skill, read [references/output-example.md](references/output-example.md).

## Self-Check

Before the final output, check that:

- The purpose of the change has not been lost in the work breakdown.
- Risks that would hurt if discovered late are exposed by early slices.
- One PR does not overmix mechanical changes, structural changes, behavior changes, migration, and release work.
- Delivery slices, PRs, deployments, and feature enablement are not confused.
- Each slice has review focus, verification method, and done condition.
- Intermediate release states, compatibility, rollback, and observability are considered.
- Judgments to raise to the tech lead are separated from judgments I can proceed with.
- Missing information is not treated as fact.
