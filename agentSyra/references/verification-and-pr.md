# Verification and Pull Requests

## Verification checklist

Choose checks from repository evidence and the approved plan. Depending on the project, run:

- Unit tests, integration tests, and focused regression tests.
- Linting, formatting, static analysis, and type checks.
- Build or compile checks.
- Migration/schema checks and authorized, bounded database assertions.
- Final diff review for logic errors, missing edge cases, dependency churn, secrets, unsafe SQL, access-control regressions, and unrelated files.

Inspect errors and fix failures that are within scope. Do not broaden scope just to hide a failing check. Do not state that tests passed if the command did not run or returned a failing result. If no executable environment exists, perform static checks where possible and mark runtime checks **not run**.

## Final diff review

Before squash and PR creation:

1. Compare the full task diff with the approved plan and acceptance criteria.
2. Inspect added, changed, renamed, and deleted files; inspect the complete staged diff, not only the diff summary.
3. Confirm no secrets or unrelated personal data are included.
4. Confirm tests and documentation are appropriate.
5. Confirm there are no accidental generated files, debug logs, temporary artifacts, or unrelated dependency updates.
6. Verify that the final one-commit history invariant is satisfied using commit comparison or commit-list evidence when available.

## Pull request

When code changes exist and the user-approved workflow requests a PR:

- Target the selected base branch.
- Use a concise, action-oriented title.
- Explain the problem and solution; summarize the main changes; list checks with exact pass/fail/not-run states; include migration, configuration, and rollout notes where relevant; list known limits.
- Use the repository's template when available.
- Confirm the returned PR URL, number, head branch, and base branch.
- Do not merge, deploy, release, or change repository settings unless separately authorized.

Mode rules:

- **Start / Interactive Plan:** open a PR after approved implementation if actual code changes exist and all PR gates pass.
- **Schedule:** each recurring run may create a PR only if that side effect was approved and the run produced changes. Never open empty PRs.
- **Review:** a read-only report does not require a PR. For approved remediation, use the same implementation, verification, squash, and PR workflow.

## Completion report

Return a compact structured summary:

1. **Outcome:** completed, partially completed, proposed, or blocked.
2. **Repository and branches:** `owner/repository`, base branch, task branch.
3. **Changes:** notable files/modules and approximate count.
4. **Verification:** exact checks with pass/fail/not run.
5. **Commit history:** confirm the number of task commits relative to base; do not claim one if not verified.
6. **Pull request:** verified URL and number if created, or exact blocker.
7. **Limitations:** meaningful uncertainty, failed tools, missing permissions, or next action.

Do not overstate certainty. Say what the available evidence proves and what remains unknown.
