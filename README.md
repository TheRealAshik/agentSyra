# Syra Agent Skill

Syra is a repository-aware coding-agent workflow for connected GitHub repositories and authorized databases.

## Repository layout

- `agentSyra/SKILL.md` — skill entry point, activation guidance, core workflow, and safety rules.
- `agentSyra/references/interaction-and-modes.md` — progressive-disclosure UI, task intake, modes, event logs, and interaction behavior.
- `agentSyra/references/implementation-workflow.md` — repository investigation, planning, approval, and implementation boundaries.
- `agentSyra/references/commit-squash-and-pr.md` — one-task-commit rule before initial PR creation, safe squash strategies, and normal post-creation update policy.
- `agentSyra/references/verification-and-pr.md` — verification, pull-request requirements, labels, and completion reporting.
- `agentSyra/references/database-safety.md` — authorized database inspection and write safety.
- `agentSyra/references/evaluation-cases.md` — expected behavior and regression test prompts.
- `agentSyra/references/release-and-version.md` — embedded skill-version reporting, official GitHub release comparison, and changelog guidance.

## System instruction options

Choose the file that matches the host environment:

- **[`SYSTEM_INSTRUCTIONS.md`](SYSTEM_INSTRUCTIONS.md)** — use when the Syra skill archive `syra-skill-package.zip` can be attached/read locally. It requires integrity checks and reading the complete packaged skill and reference set before responding.
- **[`SYSTEM_INSTRUCTION_ONLINE.md`](SYSTEM_INSTRUCTION_ONLINE.md)** — use when the agent must load the current skill from GitHub over the internet. It uses raw-file URLs, not rendered GitHub pages, and requires the entry point plus README and all seven reference documents to be fetched and read.

The canonical raw entry point used by the online variant is:
[`agentSyra/SKILL.md`](https://raw.githubusercontent.com/TheRealAshik/agentSyra/main/agentSyra/SKILL.md)

## Install/use

The checked-in skill folder is `agentSyra/`. To install it in a host that discovers skills by folder name, copy its contents into the host's `syra/` skill directory so the relative `references/` links remain intact. Keep the seven reference files alongside `SKILL.md`.

The host must provide the GitHub, database, scheduling, and interactive UI tools needed for a workflow. This repository does not itself create connectors, grant permissions, register slash commands, or install a skill into a host.

## Version and release notes

The skill embeds a development/version identifier in `agentSyra/SKILL.md` frontmatter metadata. Agents should follow [`release-and-version.md`](agentSyra/references/release-and-version.md) to compare it with official GitHub Releases and report published notes. A development identifier is not itself an official release, and no release should be inferred from a commit or version string alone.

## Maintenance

Keep both system-instruction files aligned in their operating rules. Their only intended difference is the startup source:
- `SYSTEM_INSTRUCTIONS.md` loads the full local/attached ZIP package.
- `SYSTEM_INSTRUCTION_ONLINE.md` fetches the full skill from GitHub Raw.

When updating the skill or its references, update the system-instruction files if workflow behavior changes, and keep this README's paths and descriptions in sync with the repository.
