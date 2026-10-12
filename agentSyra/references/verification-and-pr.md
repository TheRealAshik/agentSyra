# Verification and Pull Requests

## Verification checklist

Choose checks from repository evidence and the approved plan. Depending on the project, run:

- Unit tests, integration tests, and focused regression tests.
- Linting, formatting, static analysis, and type checks.
- Build or compile checks.
- Migration/schema checks and authorized, bounded database assertions.
- Final diff review for logic errors, missing edge cases, dependency churn, secrets, unsafe SQL, access-control regressions, and unrelated files.

Inspect errors and fix failures that are within scope. Do not broaden scope just to hide a failing check. Do not state that tests passed if the command did not run or returned a failing result. If no executable environment exists, perform available static/repository checks where possible and continue without treating unavailable runtime checks as a failed task step. Do not emit a standalone warning solely to announce that a test runner is unavailable; mention specific runtime checks as **not run** in the final verification summary when materially relevant.


## Structured Verification Matrix & Unexecuted Check Policy

Verification MUST distinguish between checks that were executed and checks that were skipped due to environment limitations.

### Check Categories
1. **Static Checks**: Syntax linting, type-checking, code style, static security analysis, document link checking. These should run whenever files or static tools are available.
2. **Runtime Checks**: Unit tests, integration tests, end-to-end tests, build/compile commands. Require executable environment / test runner.
3. **Environment Verification**: Database schema verification, live API endpoint checks, container runtime validation.


### Aegis Loop Verification & Self-Healing Policy
- **Synthetic Edge-Case Assertions**: Derive transient assertions directly from changed code AST/signatures to catch unhandled runtime edge cases prior to submission.
- **Bounded Repair Loop**: Allow up to 3 autonomous self-repair attempts per failing verification check before surfacing remaining blockers to the user.
- **Audit Reporting**: Record self-corrections in the task verification summary (e.g. `✅ [AEGIS] Caught null-pointer boundary condition in user.ts · Autonomously patched and verified`).

### Reporting Unexecuted Checks
- **No False Positives**: Never claim an unexecuted check passed.
- **Explicit Classification**: Mark skipped runtime checks as `[NOT RUN - Environment Limit]` rather than failed or passed.
- **No Event Log Spam**: Do not emit `⚠️ [TESTS]` error events solely because a runtime test runner is unavailable in a static workspace. Mention skipped runtime tests concisely in the final completion summary only.

## Final diff review

Before the initial PR's pre-creation squash and creation:

1. Compare the full task diff with the approved plan and acceptance criteria.
2. Inspect added, changed, renamed, and deleted files; inspect the complete staged diff, not only the diff summary.
3. Confirm no secrets or unrelated personal data are included.
4. Confirm tests and documentation are appropriate.
5. Confirm there are no accidental generated files, debug logs, temporary artifacts, or unrelated dependency updates.
6. Before the PR is first created, verify that the one-task-commit history invariant is satisfied using commit comparison or commit-list evidence. This check does not repeat for ordinary updates to an existing PR.

## Pull request

When code changes exist and the user-approved workflow requests a PR:

- Target the selected base branch.
- Use a concise, action-oriented title.
- Explain the problem and solution; summarize the main changes; list checks with exact pass/fail/not-run states; include migration, configuration, and rollout notes where relevant; list known limits.
- Use the repository's template when available.
- Confirm the returned PR URL, number, head branch, and base branch.
- For updates to an existing PR, verify the current head/base, full diff, and relevant check outcomes. New commits are allowed; do not re-squash/rewrite the branch solely to restore the pre-creation one-commit invariant.
- Do not merge, deploy, release, or change repository settings unless separately authorized.

Mode rules:

- **Start / Interactive Plan:** open a PR after approved implementation if actual code changes exist and all PR gates pass.
- **Schedule:** each recurring run may create a PR only if that side effect was approved and the run produced changes. Never open empty PRs.
- **Review:** a read-only report does not require a PR. For approved remediation, use the same implementation, verification, squash, and PR workflow.

## Automatic pull-request labels

After a pull request is created and its URL/number/head/base are verified, infer suitable labels from the approved task, changed files, linked issue metadata, and the repository's existing labeling conventions.

1. Inspect actual existing repository labels using a supported GitHub capability or verified repository evidence. Do not assume a label exists merely because it is conventional.
2. Reuse matching existing labels. Create a missing sensible label only when a connected tool explicitly supports label creation and the current permissions allow it.
3. Apply each selected label to the new PR through the available GitHub integration and verify each result. Do not claim a label was applied unless the tool confirms success.
4. If label creation is unsupported or permission-denied, apply any remaining matching labels that are available, continue the PR workflow, and report which labels could not be created or applied. Do not block PR creation solely because a label is unavailable.
5. Avoid noisy or speculative labeling. Apply only labels that match the task and repository conventions; do not invent existing labels or silently substitute unrelated ones.

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
