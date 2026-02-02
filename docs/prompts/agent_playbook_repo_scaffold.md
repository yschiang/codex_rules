You are Codex. Create a NEW repository scaffold that enforces an agent-driven engineering playbook.

# Goal
Create a repo that contains:
1) Long-term rules (Coding / Arch guardrails / Test quality / Security / PR policy)
2) Workflows (Bugfix / Feature / Refactor) with Plan/Act mode
3) Skills (repro, root cause, red test, minimal fix, regression+observability, PR summary) that map to workflow steps
4) Artifact templates (Change Plan, PR Summary)

This repo is a “playbook repo” (no product code required). Make it clean, auditable, and easy to extend.

# Output requirements
- Produce the full directory structure + file contents.
- Every file must be complete and copy-pastable.
- Use Markdown for all docs.
- Keep rules concise but enforceable (no vague guidance).
- Avoid referencing any proprietary/internal system names or secrets.

# Repository structure (MUST create exactly this)
.
├─ README.md
├─ .clinerules
├─ clinerules/
│  ├─ 00_index.md
│  ├─ 10_coding-standards.md
│  ├─ 20_arch-guardrails.md
│  ├─ 30_testing-quality.md
│  ├─ 40_security.md
│  ├─ 50_pr-and-release.md
│  ├─ 61_workflow_bugfix.md
│  ├─ 62_workflow_feature.md
│  ├─ 63_workflow_refactor.md
│  ├─ 70_output-artifacts.md
│  └─ 80_definition-of-done.md
└─ ai/
   ├─ README.md
   ├─ guidelines.md
   ├─ workflow.md
   ├─ templates/
   │  ├─ change_plan.md
   │  └─ pr_summary.md
   └─ skills/
      ├─ 01_repro_min.md
      ├─ 02_root_cause.md
      ├─ 03_write_failing_test.md
      ├─ 04_minimal_fix.md
      ├─ 05_regression_plus_observability.md
      └─ 06_pr_ready_summary.md

# Content rules (MUST)
## A) Long-term rules (clinerules/*.md)
- 10_coding-standards.md: minimal diff, no drive-by refactor, dependency discipline, error handling basics
- 20_arch-guardrails.md: contract stability, boundaries, backward compatibility, performance guardrails, observability placement
- 30_testing-quality.md: bugfix red->green requirement, deterministic tests, test level selection, evidence requirements
- 40_security.md: secrets ban, input validation, logging safety, dependency caution, security regression tests
- 50_pr-and-release.md: PR content checklist, risk/rollback, commit hygiene

## B) Workflows (Plan/Act mode)
- In clinerules/00_index.md define:
  - load order
  - Plan mode output format
  - Act mode rule: one small step per response + result + next step
  - Skill invocation mapping
- Bugfix workflow must enforce:
  1) repro (prefer failing test)
  2) root cause with file/function/condition
  3) failing test (must be red)
  4) minimal fix (avoid refactor)
  5) regression + minimal observability only if needed
  6) PR-ready summary
- Feature workflow: acceptance criteria -> tests -> minimal implementation -> verify -> PR summary
- Refactor workflow: invariants -> characterization tests if needed -> refactor minimal diff -> verify -> PR summary

## C) Skills must produce artifacts
- Each ai/skills/*.md defines:
  - Purpose
  - Inputs
  - Steps
  - Outputs (explicit artifacts)
- Skills must align with the workflows; workflows must reference skills.

## D) Artifacts templates
- ai/templates/change_plan.md must include: goal, scope(in/out), steps, files, test strategy, risks & rollback
- ai/templates/pr_summary.md must include: issue/goal, root cause or design intent, fix/changes, tests(commands+results), risk & rollback

## E) ai/guidelines.md, ai/workflow.md
- guidelines.md: short summary of non-negotiables and how rules relate to clinerules
- workflow.md: describe the general process and required deliverables across workflows

# README.md (MUST)
Explain:
- What this repo is
- How to use with an agent (Cline/Codex-style)
- Where rules/workflows/skills/templates are
- How to add a new workflow and a new skill

# Final response format
1) Print the directory tree
2) Then print each file in full content, in the same order as the tree
3) No extra commentary beyond what’s needed to understand the scaffold
