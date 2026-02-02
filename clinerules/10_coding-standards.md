# Coding Standards

## Minimal Diff
- Change only what is required to meet the goal.
- Prefer targeted edits over broad rewrites.

## No Drive-By Refactors
- Do not rename, reformat, or reorganize unrelated code.
- Any refactor must be tied to the explicit task and justified in the change plan.

## Dependency Discipline
- Avoid new dependencies unless explicitly required.
- Prefer standard library and existing project utilities.
- Record any new dependency and its purpose in the PR summary.

## Error Handling Basics
- Validate inputs at boundaries.
- Return actionable errors with context, without leaking sensitive data.
- Do not swallow errors; handle or propagate intentionally.
