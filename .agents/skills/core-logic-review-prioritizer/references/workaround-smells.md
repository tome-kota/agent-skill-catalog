# Workaround Smell Signals

AI-assisted changes often produce code that works locally but degrades structure globally. Use this file to look for those cases.

## Common Signals

- the same decision logic is duplicated across multiple layers
- special-case handling is placed outside its natural responsibility
- helper extraction happened, but the real business or control decision became harder to locate
- only the exception path was patched locally, without re-shaping the overall rule
- states or flags increased, but the overall transition model was not cleaned up
- a one-off condition is hidden behind abstraction
- names or comments suggest `temporary`, `workaround`, `fallback`, or `quick fix`
- authorization decisions are duplicated in multiple places
- retry or replay conditions are distributed across several components
- consistency problems are hidden behind feature flags or temporary flags
- audit events or traces are missing only on failure paths
- fallback or failover decisions are scattered across responsibility boundaries
- control decisions are split across jobs, workers, services, and controllers

## Reviewer Questions

- where should this decision naturally live
- is the same rule written again somewhere else
- is this exception handling a real rule extension or just patchwork
- will the next similar change be easier after this one
- is the touched area easier to understand than before
- can this authorization or control decision be explained from one place
- are failure handling and retry handling split into a separate path for no good reason
- are auditing, tracing, or notification split between normal and failure flows

## Things Commonly Mistaken For Improvement

- helper extraction only
- adding interfaces only
- “preparing for future extensibility” when the code really just hides a one-off condition
- fewer lines of code, but less visible decision ownership
