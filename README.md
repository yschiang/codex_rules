# Agent Engineering Playbook Repo

## What This Repo Is
A clean, auditable playbook repository that defines durable engineering rules, agent workflows, skills, and artifact templates. It contains no product code. It is designed to standardize how human and agent collaborators plan, execute, and document work.

## How To Use With An Agent
1. Point your agent to this repo as its working directory.
2. Have it load `clinerules/00_index.md` first, then follow the load order listed there.
3. Choose a workflow (`bugfix`, `feature`, or `refactor`) and follow its required steps and artifacts.
4. Use the skills in `ai/skills/` to produce the required artifacts and outputs.

This structure is compatible with Cline/Codex-style agents that read repository rules and follow Plan/Act execution.

## Where Things Live
- Rules and workflows: `clinerules/`
- AI guidelines and general workflow: `ai/guidelines.md`, `ai/workflow.md`
- Skills: `ai/skills/`
- Templates: `ai/templates/`

## How To Add A New Workflow
1. Add a new workflow file in `clinerules/` (e.g., `64_workflow_docs.md`).
2. Update `clinerules/00_index.md` to include it in the load order and skill mapping.
3. Define the workflow steps and required artifacts, referencing skills.
4. If new artifacts are needed, add templates in `ai/templates/`.

## How To Add A New Skill
1. Create a new skill file in `ai/skills/` with Purpose, Inputs, Steps, Outputs.
2. Update the relevant workflow to reference the new skill.
3. Update `clinerules/00_index.md` skill mapping.

