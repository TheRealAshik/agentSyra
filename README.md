# Syra Agent Skill

Syra is a repository-aware coding agent workflow for connected GitHub repositories and authorized databases.

## Package contents

- `SKILL.md` — concise entrypoint, triggers, required workflow, and safety rules.
- `references/interaction-and-modes.md` — progressive-disclosure UI behavior, repository/branch selection, and four operating modes.
- `references/implementation-workflow.md` — repository inspection, plans, approval, and implementation boundaries.
- `references/commit-squash-and-pr.md` — required one-task-commit invariant before PR creation, including safe squash fallbacks.
- `references/verification-and-pr.md` — test checks, final diff review, PR requirements, and completion report.
- `references/database-safety.md` — database access and write safety.
- `references/evaluation-cases.md` — expected behavior test prompts.

## Install/use

Copy the `syra/` folder into the skills directory supported by your agent host. The host must support Agent Skills discovery and provide the GitHub/database/scheduler/UI tools needed for each workflow. This package does not itself create connectors, grant permissions, register slash commands, or install itself.

## Interaction style

Syra uses structured, factual event logs (for example `✅ [GITHUB] Connection verified`) plus progressive disclosure: show only the selector/question/decision needed now. Logs must report verified results, not explain future steps. Interactive Plan runs a thorough adaptive requirements interview with option-based decisions, conditional follow-ups, a completeness/contradiction check, and explicit approval before implementation. It asks about every relevant unresolved dimension without repeating questions or demanding irrelevant choices. The evaluation cases test startup logs and detailed interactive planning.

## Key commit policy

Before creating a pull request, Syra should ensure the task branch contains exactly one Syra task commit relative to the selected base branch. Prefer making one multi-file commit from the start. If the connector creates several commits, inspect the full range and safely squash only task-owned commits before PR creation. If this cannot be done safely with available tools, report the limitation rather than silently opening a multi-commit PR.

## Format notes

The package uses the open Agent Skills `SKILL.md` structure with YAML frontmatter and relative references. Validate against your target host's supported version before distribution if that host adds stricter conventions.

Official format specification: https://agentskills.io/specification
