# Security / Authorization / Auditability Governance

## Purpose

Review whether a design respects security constraints, authorization boundaries, trust boundaries, and auditability requirements before those concerns become expensive to retrofit.

## When to use

Use this reference when a proposal changes who can do what, crosses tenant or trust boundaries, handles sensitive data, introduces privileged workflows, or affects logging and incident investigation.

## Primary questions

- What assets and actions need protection?
- Where does trust change, and where must enforcement happen?
- What authorization, revocation, and audit behavior is required?
- Which hidden trust assumptions make the design unsafe?

## Review procedure

1. Identify protected assets and high-impact actions.
2. Map trust boundaries and enforcement points.
3. Review authorization rules, revocation behavior, and sensitive data exposure.
4. Review auditability, failure behavior, and abuse paths.
5. State the design corrections required to satisfy the security constraints.

## Output cues

Produce a review that names the violated constraint, the missing enforcement or traceability behavior, and the design correction needed.

## Review stance

Do not reduce security review to checklist theater.

The important question is not "did we mention auth?" It is:

- where is trust being assumed?
- who can really do what?
- what cannot be reconstructed after the fact?

## Protected-asset checks

Identify:

- sensitive data
- privileged actions
- irreversible operations
- tenant-crossing behavior
- regulated or high-trust workflows

If the design cannot name what must be protected, it is not ready for serious review.

## Trust-boundary checks

Look for boundary changes such as:

- browser to backend
- public API to internal service
- one service to another
- tenant to tenant
- admin to user scope
- automation to privileged system behavior

Security problems often live at the place where trust silently changes.

## Authorization checks

Ask:

- what rule decides permission?
- where is that rule enforced?
- can object-level access be bypassed?
- how is access revoked?

Hidden UI is not authorization. Internal network placement is not authorization.

## Auditability checks

Review whether the system can reconstruct:

- who acted
- what changed
- when it happened
- why it happened
- whether the audit trail itself can be trusted

If the system cannot explain sensitive behavior, the design is incomplete.

## Common security design traps

### Frontend-only enforcement

The UI is doing more trust work than the server boundary.

### Implicit internal trust

"Internal" is treated as equivalent to "safe."

### Missing object-level authorization

The user can access a class of resource, but the design never checks whether they may access this resource.

### Audit afterthought

Mutation is modeled, but traceability is bolted on later.

### Revocation blind spot

The design can grant access but has no strong model for removing it.

## Deep checks

Use this section when the initial review suggests that the design mentions security but may still be trusting the wrong boundary or missing failure cases.

### What to probe further

- whether object-level authorization is explicit or merely assumed
- whether "internal" systems are being trusted without real enforcement
- whether revocation, suspension, or emergency access removal has a concrete path
- whether the audit trail can survive failure, tampering, or delayed ingestion

### Strong warning signs

- the UI hides an action, so the team treats it as protected
- one service validates access but downstream services assume the result forever
- privileged actions are audited only after success, not around the full attempt
- tenant boundaries rely on conventions rather than enforceable checks

### Review prompts

- Where does trust actually change in this flow?
- What action can the wrong actor still perform if one assumption breaks?
- How is access removed, not just granted?
- What important event would be hard to reconstruct after the fact?

## Recommendation rules

- Move security-relevant checks to trusted boundaries.
- Make object-level authorization explicit when resource ownership matters.
- Add auditability as part of the design contract, not a logging afterthought.
- Treat revocation and failure behavior as core design requirements.
- Reject convenience when it violates a named security constraint.

## Output shape

Structure findings around:

- protected assets and trust boundaries
- missing or weak enforcement points
- auditability or revocation gaps
- the required design corrections

## Acceptance checks

A strong review from this reference should:

- identify protected assets and trust boundaries
- make authorization rules and enforcement points explicit
- include auditability and revocation in the judgment
- find hidden trust assumptions
- recommend concrete security design corrections
