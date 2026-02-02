# Architecture Guardrails

## Contract Stability
- Do not change public interfaces without explicit approval and versioning guidance.
- Preserve existing API shapes and semantics unless the task requires change.

## Boundaries
- Respect module boundaries and layering.
- Avoid cross-layer shortcuts and hidden couplings.

## Backward Compatibility
- Existing callers must continue to work unless explicitly noted.
- If breaking change is required, document migration steps in the change plan.

## Performance Guardrails
- Avoid new hot-path allocations or O(n^2) operations without justification.
- If performance may regress, measure and document impact.

## Observability Placement
- Add logs/metrics at entry points, error paths, or state transitions.
- Do not add high-volume logging in loops or hot paths.
