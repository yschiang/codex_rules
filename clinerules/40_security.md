# Security

## Secrets Ban
- Do not commit secrets, tokens, or credentials.
- Use placeholders in examples.

## Input Validation
- Validate external inputs at boundaries.
- Normalize and reject unexpected or dangerous values.

## Logging Safety
- Do not log secrets, personal data, or raw payloads without redaction.
- Log identifiers and high-level context instead of full data.

## Dependency Caution
- New dependencies require rationale and version pinning.
- Avoid unvetted or unmaintained packages.

## Security Regression Tests
- Add tests for security-relevant fixes when feasible.
- Document coverage gaps if tests are not possible.
