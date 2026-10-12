# Syra Agent Skill

Syra is a repository-aware coding-agent workflow for connected GitHub repositories and authorized databases.

## Repository layout

- `agentSyra/SKILL.md` — canonical skill entry point, activation guidance, core workflow, safety rules, and the complete reference index.
- `agentSyra/references/interaction-and-modes.md` — progressive-disclosure UI, task intake, modes, event logs, and interaction behavior.
- `agentSyra/references/implementation-workflow.md` — repository investigation, planning, approval, and implementation boundaries.
- `agentSyra/references/commit-squash-and-pr.md` — one-task-commit rule before initial PR creation, safe squash strategies, and normal post-creation update policy.
- `agentSyra/references/verification-and-pr.md` — verification, pull-request requirements, labels, and completion reporting.
- `agentSyra/references/database-safety.md` — authorized database inspection and write safety.
- `agentSyra/references/evaluation-cases.md` — expected behavior and regression test prompts.
- `agentSyra/references/release-and-version.md` — embedded skill-version reporting, official GitHub release comparison, and changelog guidance.

## System instruction

Use **[`SYSTEM_INSTRUCTIONS.md`](SYSTEM_INSTRUCTIONS.md)** as the sole default system instruction. It fetches and reads the canonical skill entry point from GitHub Raw. The system instruction deliberately does not duplicate individual reference URLs: `agentSyra/SKILL.md` owns the reference index and directs the agent to task-relevant guidance.

Canonical raw entry point:
[`agentSyra/SKILL.md`](https://raw.githubusercontent.com/TheRealAshik/agentSyra/main/agentSyra/SKILL.md)

## Install/use

The checked-in skill folder is `agentSyra/`. To install it in a host that discovers skills by folder name, copy its contents into the host's `syra/` skill directory so the relative `references/` links remain intact. Keep the seven reference files alongside `SKILL.md`.

The host must provide the GitHub, database, scheduling, and interactive UI tools needed for a workflow. This repository does not itself create connectors, grant permissions, register slash commands, or install a skill into a host.

## Version and release notes

The skill embeds a development/version identifier in `agentSyra/SKILL.md` frontmatter metadata. Agents should follow the reference index in `agentSyra/SKILL.md`, including `release-and-version.md`, to compare it with official GitHub Releases and report published notes. A development identifier is not itself an official release, and no release should be inferred from a commit or version string alone.

## Maintenance

Maintain the reference index in `agentSyra/SKILL.md` when adding, removing, or renaming reference documents. Keep detailed workflow guidance in the relevant reference files. Do not duplicate the reference inventory in `SYSTEM_INSTRUCTIONS.md` or create a second system-instruction variant.
