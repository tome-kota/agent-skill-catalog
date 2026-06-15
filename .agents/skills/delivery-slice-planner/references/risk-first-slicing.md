# Risk-First Slicing

## Purpose

Expose uncertainty that would be expensive if discovered late through early slices.

Risk-first slicing does not mean "build all the dangerous parts first." It means creating small, testable pieces that quickly produce evidence for design, implementation, and operational decisions.

## When To Use

- External integrations, authorization, performance, data integrity, migration, or backward compatibility are uncertain.
- The design looks plausible, but feasibility cannot be known until implementation is attempted.
- If discovered late, the issue could disrupt PR splitting or the release plan.
- Evidence is needed early for tech-lead judgment.

## Risk Types

- `Contract risk`: APIs, DTOs, events, DB schemas, or external interfaces may break existing consumers.
- `Data risk`: Migration, consistency, duplication, missing data, reruns, or rollback may be difficult.
- `Authorization risk`: It is unclear who can see, operate, or audit what.
- `State risk`: Statuses, modes, transitions, and failure handling are complex.
- `Performance risk`: Volume, frequency, synchronous processing, N+1 behavior, external calls, or batch duration are uncertain.
- `Operational risk`: Investigation, recovery, reruns, or support handling may be difficult during incidents.
- `Organizational risk`: Multiple teams, the tech lead, operations, or customer confirmation are involved.

## Procedure

1. List the risks included in the change.
2. Estimate how painful each risk would be if discovered late.
3. Look for the smallest way to verify each risk.
4. Do not overmix risk-exposure slices with value-delivery slices or structural-change slices.
5. State how the verification result changes the later slices.

## Good Risk-First Slices

- Easy to discard if they fail.
- Do not break production users.
- Produce evidence that can guide later decisions.
- Make clear what the tech lead or reviewers should confirm.
- Avoid excessive generalization even if the work can remain as part of the final implementation.

## Bad Risk-First Slices

- They become "build the whole foundation first."
- They are large preparation work with no value and no verification.
- They do not affect later design decisions.
- They ship schema or contract changes that cannot be safely rolled back before verification.
- They pretend to address risk while avoiding the most uncertain part.

## Review Questions

- What would be the most painful thing to discover late?
- What is the smallest thing that can expose it?
- Which later decision changes based on this slice's result?
- Is this verification meaningful under conditions close to production data, production load, or production authorization?
- If it fails, can the team discard, roll back, or redo it?

## Output Integration

In `Slicing Strategy`, write the main risks and the order in which they will be exposed.

In `Proposed Slices`, include the following for risk-first slices:

- Risk to expose
- Verification method
- Verification done condition
- Whether the intermediate state after verification may exist in production
- Follow-up plan if successful
- Revision direction if unsuccessful
- Judgment to confirm with the tech lead
