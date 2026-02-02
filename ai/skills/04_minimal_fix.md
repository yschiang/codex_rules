# Skill: Minimal Fix

## Purpose
Apply the smallest change that makes the failing test pass without refactors.

## Inputs
- Root Cause Report
- Failing Test artifact
- Relevant code files

## Steps
1. Implement a targeted change that addresses the exact condition.
2. Avoid renames, formatting changes, or unrelated cleanup.
3. Run the failing test to confirm it passes.

## Outputs
- Fix Notes artifact with files changed and rationale.
- Green Test Evidence with command and result.
