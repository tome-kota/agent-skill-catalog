# Quality Model

Use this file as the hidden rubric for selecting concerns. Usually read only the sections that are relevant. Do not expose characteristic or subcharacteristic names directly to the user unless there is a clear reason.

## Table of Contents

- Functional suitability
- Performance efficiency
- Compatibility
- Usability
- Reliability
- Security
- Maintainability
- Portability

## Functional Suitability

Subcharacteristics:

- Functional completeness
- Functional correctness
- Functional appropriateness

What to inspect:

- What counts as success
- Whether expected behavior is defined not only for the happy path but also for boundary conditions
- Whether this feature is solving the right problem with the right scope

Translation examples:

- What conditions should count as "working as expected"?
- How should the system behave for edge cases or unexpected input?
- Which cases do you want this feature to handle, and which cases should stay out of scope for now?

## Performance Efficiency

Subcharacteristics:

- Time behavior
- Resource utilization
- Capacity

What to inspect:

- Required response time or processing time
- Constraints on CPU, memory, storage, or network usage
- Assumptions about concurrency, data volume, and peak load

Translation examples:

- What level of response time or processing time do you expect?
- What scale of data volume or concurrent usage should I assume?
- Would you rather prioritize speed even if the implementation becomes more complex, or is readability more important here?

## Compatibility

Subcharacteristics:

- Co-existence
- Interoperability

What to inspect:

- Whether this can coexist safely with existing systems or callers
- Whether this preserves contracts and data shapes with integrations

Translation examples:

- Do we need to avoid impact on existing callers or integrations?
- Must the input/output format or API contract stay unchanged?
- Is there anything in current operational flow or nearby features that could conflict with this change?

## Usability

Subcharacteristics:

- Appropriateness recognizability
- Learnability
- Operability
- User error protection
- User interface aesthetics
- Accessibility

What to inspect:

- Whether users can understand and use the feature without confusion
- Whether error-prone interactions should be reduced
- Whether clarity of UI or messaging is important
- Whether there are accessibility requirements

Translation examples:

- What is the most important thing to make clear for the people using this feature?
- Are there any likely user mistakes that we should specifically guard against?
- Are there any rules for wording or interaction flow that we should preserve?
- Are there accessibility conditions we should account for?

## Reliability

Subcharacteristics:

- Maturity
- Availability
- Fault tolerance
- Recoverability

What to inspect:

- What should happen when failures occur
- How resistant to interruption the behavior should be
- How to think about partial failure, retry, and recovery

Translation examples:

- If this fails, should we just return an error, or should we plan for retry or recovery?
- If only part of the process fails, how much should continue?
- Do we need this to remain safe under re-execution or duplicate execution?

## Security

Subcharacteristics:

- Confidentiality
- Integrity
- Non-repudiation
- Accountability
- Authenticity

What to inspect:

- Who should be allowed to do what
- Whether unauthorized access or tampering must be prevented
- Whether auditability, traceability, or authentication requirements exist

Translation examples:

- Who should be allowed to perform this action?
- Is there any information here that must not be exposed, or data that must not be tampered with?
- Do we need to retain an execution or change history?
- Are there operations that must be restricted to authenticated users only?

## Maintainability

Subcharacteristics:

- Modularity
- Reusability
- Analysability
- Modifiability
- Testability

What to inspect:

- Whether this should withstand future rule additions and future changes
- Whether root-cause investigation and testing ease matter here
- Whether the structure should support local changes without broad fallout

Translation examples:

- Do you expect more conditions or exception rules like this in the future?
- Is this likely to be an area we will keep changing?
- Is there any part here that you especially want to keep easy to trace through logs or tests?
- Which matters more here: the smallest short-term fix or making future changes easier?

## Portability

Subcharacteristics:

- Adaptability
- Installability
- Replaceability

What to inspect:

- Whether the feature must adapt across multiple environments
- Whether deployment or setup complexity is constrained
- Whether future replacement of implementation or platform should remain possible

Translation examples:

- Does this need to work consistently across multiple environments?
- Are there constraints on deployment steps or setup complexity?
- Is there a real chance that we will swap this implementation or its underlying platform later?
