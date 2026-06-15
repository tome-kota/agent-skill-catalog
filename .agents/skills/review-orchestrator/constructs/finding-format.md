# Finding Format

Use this compact format for every finding:

```md
## Finding

- ID:
- Perspective:
- Severity: Blocking | Warning | Note
- Confidence: High | Medium | Low
- Location:
- Evidence:
- Problem:
- Risk:
- Recommendation:
- Required action:
```

Rules:

- Assign stable IDs such as `ARCH-1`, `IMPL-1`, `SEC-1`, `TEST-1`, or `MAINT-1`.
- For code or diff review, cite concrete files, symbols, code snippets, diff hunks, or line locations.
- For plan review, cite plan sections, headings, assumptions, missing artifacts, proposed steps, or unstated constraints.
- For PR or issue review, cite PR sections, issue text, linked artifacts, commit ranges, or missing evidence.
- Use `Location: Missing artifact` only when the absence itself is the evidence.
- Do not produce vague findings.
- Do not report issues without an actionable recommendation.
- Prefer fewer high-signal findings over many weak comments.
- If no issue is found for a perspective, return `No findings`.
