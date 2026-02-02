# SpringBoot Migration Rules

## Version Targeting
- Define a single target Spring Boot version and record it in the change plan.
- Do not mix multiple target versions in one migration.

## Dependency Alignment
- Use the Spring Boot dependency management or BOM to control versions.
- Remove explicit dependency versions that are managed by the BOM unless required.
- Record any dependency version overrides and why they are needed.

## Configuration Migration
- Inventory configuration keys and update renamed or removed properties.
- Keep `application-*.yml` or `application-*.properties` changes minimal and scoped.

## API And Behavior Compatibility
- Verify public endpoints and message formats remain compatible unless explicitly changed.
- If behavior changes are required, document user-visible impacts and migration steps.

## Build And Runtime Baselines
- Record the Java version and build tool version before and after migration.
- Ensure the application starts cleanly with no new warnings or errors.

## Test And Observability
- Add or update tests for any behavior changes.
- Add minimal startup or health checks only when needed for confidence.
