# Testing Quality

## Bugfix Red To Green
- A bugfix must start with a failing test or a deterministic repro.
- The test must be red before the fix and green after.

## Deterministic Tests
- Tests must be stable and repeatable.
- Avoid time, randomness, and network unless controlled.

## Test Level Selection
- Prefer the lowest-level test that proves the behavior.
- Use integration or end-to-end tests only when unit tests cannot validate the behavior.

## Evidence Requirements
- Record the exact test command and result in the PR summary.
- If tests are skipped, document why and what evidence substitutes.
