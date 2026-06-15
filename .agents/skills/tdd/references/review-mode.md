# Review Mode

Review mode checks whether an implementation, pull request, plan, or AI-generated code preserves the TDD chain. It is a mode integrated into `/tdd`, not a separate skill.

Do not look only at the final test file. Check the sequence:

- Which production behavior was chosen
- Which public interface or stable boundary was used
- Whether there was an observed failing behavior test
- Whether the red was dirty red or clean red
- Whether production logic waited for clean red
- Whether the implementation was minimal green
- Whether refactoring happened only while green
- Whether mocks were justified as real boundaries

## Severity

Blocker:

- New or changed expected behavior entered production code without an observed red
- A post-hoc test is being presented as TDD
- A test claimed as the red for a new or changed expected behavior passes immediately

Major:

- Implementation-detail tests
- Unnecessary internal mocks
- Overimplementation beyond the current behavior
- Refactoring mixed with behavior changes
- Using dirty red as permission to add real logic

Minor:

- Unclear test names
- Multiple behaviors packed into one test
- Missing evidence summary

Judgment labels:

- `TDD chain unproven`
- `Partially verified`
- `Verified`

Application rules:

- If there is no sequence evidence, use `TDD chain unproven`
- If only red can be confirmed, use `Partially verified` and write `red only` in the evidence field
- If only green can be confirmed, use `Partially verified` and write `green only` in the evidence field
- If the target cycle includes red, minimal green, and observed green, use `Verified`
- If refactoring happened, use `Verified` only when re-green can also be confirmed

## Output Format

Use this format for each finding:

```text
Finding:
Severity:
Judgment:
Broken TDD principle:
Evidence:
Risk:
Recovery instruction:
```

Recovery instructions should give concrete steps the implementer can use to restore the TDD chain. Do not stop at criticism.

## Review Guidance

Prioritize concrete evidence:

- Commit order or diff order, when available
- Test output showing observed red or green
- Test names and assertions
- Production code that goes beyond the current behavior
- Mocks that verify internal collaboration instead of behavior

If sequence evidence is unavailable, say so clearly and review the artifacts that do exist. Do not claim TDD happened just because tests exist.
