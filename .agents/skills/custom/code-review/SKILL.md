---
name: code-review
description: Review a proposed code change for correctness, scope, safety, concurrency, resources, performance, compatibility, tests, and maintainability without assigning a quality score.
---

# General Code Review

Use this skill when reviewing a diff, a completed change, or a focused implementation. Review the actual change against the requested behavior and surrounding code, not an imagined ideal rewrite.

## Review approach

1. Read the task, relevant project instructions, and the complete diff.
2. Inspect affected callers, consumers, data structures, configuration, and error paths when the diff changes them.
3. Check both the normal path and meaningful failure, boundary, concurrency, and cleanup paths.
4. Confirm that tests or other verification cover the changed behavior and important regressions.
5. Report only actionable findings supported by code or repository evidence.

## Checklist

Check:

- requirement satisfaction and behavior correctness;
- accidental scope expansion or unrelated changes;
- regressions in existing behavior and compatibility;
- input validation, permissions, sensitive data, injection, and other security risks;
- error handling, failure propagation, retries, and partial failure;
- concurrency, ordering, shared state, and race-prone lifecycle behavior;
- resource acquisition and release;
- performance implications supported by the changed code and likely data paths;
- tests, observability, documentation, and maintainability.

## Severity

Use these labels:

- Critical: severe security, data integrity, availability, or correctness failure that can make the change unsafe to ship.
- High: likely serious production failure, compatibility break, or major missing requirement.
- Medium: meaningful bug or risk with a narrower impact or a practical workaround.
- Low: limited correctness, maintainability, or robustness issue worth addressing.

Do not assign a numeric score or a general quality grade.

## Finding format

For each finding, include:

- severity;
- precise file and line or code location;
- the observed behavior or defect;
- why it matters;
- a focused fix direction when useful.

Order findings from highest to lowest severity. Keep each finding focused on one issue.

## No-findings result

If no actionable issue is found, say so explicitly and list the areas and paths that were checked. Distinguish “no issue found in the reviewed scope” from “all possible issues are ruled out.”
