# Index And Execution Rules

## Load Order
1. `clinerules/10_coding-standards.md`
2. `clinerules/20_arch-guardrails.md`
3. `clinerules/25_springboot-migration.md`
4. `clinerules/30_testing-quality.md`
5. `clinerules/40_security.md`
6. `clinerules/50_pr-and-release.md`
7. `clinerules/61_workflow_bugfix.md`
8. `clinerules/62_workflow_feature.md`
9. `clinerules/63_workflow_refactor.md`
10. `clinerules/64_workflow_springboot-migration.md`
11. `clinerules/70_output-artifacts.md`
12. `clinerules/80_definition-of-done.md`

## Plan Mode Output Format
Use this exact format:

```
PLAN
- Goal:
- Assumptions:
- Steps:
- Risks:
- Artifacts:
```

## Act Mode Rule
One small step per response. Each response must include:
1. The step performed.
2. The result (what changed, what was observed).
3. The next step.

## Skill Invocation Mapping
- Bugfix workflow steps map to `ai/skills/01_repro_min.md`, `ai/skills/02_root_cause.md`, `ai/skills/03_write_failing_test.md`, `ai/skills/04_minimal_fix.md`, `ai/skills/05_regression_plus_observability.md`, `ai/skills/06_pr_ready_summary.md`.
- Feature workflow steps map to `ai/skills/03_write_failing_test.md`, `ai/skills/04_minimal_fix.md`, `ai/skills/05_regression_plus_observability.md`, `ai/skills/06_pr_ready_summary.md`.
- Refactor workflow steps map to `ai/skills/02_root_cause.md`, `ai/skills/03_write_failing_test.md`, `ai/skills/04_minimal_fix.md`, `ai/skills/05_regression_plus_observability.md`, `ai/skills/06_pr_ready_summary.md`.
- SpringBoot migration workflow steps map to `ai/skills/07_springboot_migration_assess.md`, `ai/skills/08_springboot_migration_plan.md`, `ai/skills/09_springboot_migration_execute.md`, `ai/skills/10_springboot_migration_verify.md`, `ai/skills/06_pr_ready_summary.md`.
