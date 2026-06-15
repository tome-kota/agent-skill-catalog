---
name: review-orchestrator
description: Orchestrate exactly five isolated code review perspectives for AI-generated or human-authored changes. Use when reviewing pull requests, diffs, implementation plans, or coding-agent output for architecture boundaries, implementation integrity, security and authorization, test integrity, convention consistency, maintainability, and AI implementation anti-patterns such as fallback implementations, ad-hoc patches, type weakening, compatibility creep, and responsibility leakage.
---

# Review Orchestrator

Run review as five narrow, explicit, repeatable perspectives. Each review perspective must be narrow and complete. The orchestrator must ensure all five perspectives are executed.

## Execution Protocol

1. Identify the review artifact: diff, PR, plan, commit range, files, or user-provided patch.
2. Read the shared constructs for every perspective pass:
   - [constructs/finding-format.md](constructs/finding-format.md)
   - [constructs/severity.md](constructs/severity.md)
   - [constructs/review-execution-contract.md](constructs/review-execution-contract.md)
3. Execute all five perspective files:
   - [reviews/architecture-boundary.md](reviews/architecture-boundary.md)
   - [reviews/implementation-integrity.md](reviews/implementation-integrity.md)
   - [reviews/security-authorization.md](reviews/security-authorization.md)
   - [reviews/test-integrity.md](reviews/test-integrity.md)
   - [reviews/convention-maintainability.md](reviews/convention-maintainability.md)
4. Keep each perspective isolated. Use forked context, subagents, or custom agents when available.
5. If isolation support is unavailable, run the perspectives sequentially with input isolation:
   - start each perspective from the shared artifact, shared constructs, and that perspective file only
   - do not read earlier perspective findings while producing the current perspective
   - do not use another perspective's finding as evidence
   - write each perspective result before reading the next perspective file
6. Do not skip, combine, or shorten a perspective because another perspective found issues.
7. Require each perspective to return findings in the shared format, or `No findings`.
8. Aggregate by perspective. Put blocking IDs in the summary first, then keep detailed findings under their perspective.
9. Include a completion checklist for all five perspectives. If any perspective could not execute, state why.
10. Read [constructs/output-example.md](constructs/output-example.md) only when formatting the final aggregated result. Do not use it as input to any perspective pass.

Checklist rule:

- use `- [x]` for a completed perspective
- use `- [ ]` only for a perspective that was not executed
- when a perspective was not executed, add the reason inline, such as `- [ ] Security / Authorization - not executed: artifact missing`

Overall status rule:

- `Blocked`: one or more Blocking findings.
- `Needs changes`: no Blocking findings, but one or more Warning findings.
- `Looks acceptable`: no findings, or only Note findings.

## Final Output

Use this shape:

```md
# Review Result

## Summary

- Overall status: Blocked | Needs changes | Looks acceptable
- Blocking findings by ID:
- Warning count:
- Note count:

## Perspective Completion

- [x] Architecture / Boundary
- [x] Implementation Integrity
- [x] Security / Authorization
- [x] Test Integrity
- [x] Convention / Maintainability

## Findings by Perspective

### 1. Architecture / Boundary

### 2. Implementation Integrity

### 3. Security / Authorization

### 4. Test Integrity

### 5. Convention / Maintainability

## Recommended Next Actions
```
