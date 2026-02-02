# Workflow: Bugfix

## Required Sequence
1. Reproduce the bug, preferably with a failing test. Use `ai/skills/01_repro_min.md`.
2. Identify root cause with file, function, and condition. Use `ai/skills/02_root_cause.md`.
3. Write a failing test that is red. Use `ai/skills/03_write_failing_test.md`.
4. Apply a minimal fix and avoid refactors. Use `ai/skills/04_minimal_fix.md`.
5. Add regression coverage and minimal observability only if needed. Use `ai/skills/05_regression_plus_observability.md`.
6. Produce a PR-ready summary. Use `ai/skills/06_pr_ready_summary.md`.

## Required Artifacts
- Change Plan
- PR Summary

## Stop Conditions
- If reproduction is not possible, record evidence and stop.
- If root cause cannot be determined, stop and request escalation.
