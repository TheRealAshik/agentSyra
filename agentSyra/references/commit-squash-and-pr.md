# Commit, Squash, and PR Policy

## Required result

**Before a new PR is created, the task branch should contain exactly one Syra task commit relative to the selected base branch.** That commit should capture the complete approved change set. This keeps PR history readable even if the connector emitted one commit per edited file.

The default is still to avoid unnecessary intermediate commits before the first PR: accumulate related edits, verify them together, and create one task-level commit. Squashing is the final normalization step only when multiple Syra task commits exist before that initial PR is created. It is not a recurring requirement for later updates to an existing PR.

## Preferred order of approaches

### 1. Workspace-backed Git

1. Start from the selected base and a Syra-owned task branch.
2. Edit all relevant files in the working tree without committing each file.
3. Inspect `git status`, the full diff, and the set of staged/unstaged paths. Stage only intended task files.
4. Run relevant checks and fix in-scope failures before the task commit.
5. Create one concise task-level commit.
6. Before PR creation, confirm there is exactly one task commit relative to the selected base.

If several Syra-only commits already exist in the branch, first confirm the branch range contains no unrelated or user-authored commits and all changes are intended. On a local, Syra-owned branch, a soft reset to the verified base followed by a new commit can consolidate the staged diff; inspect the staged diff before writing the replacement commit. Do not run reset/rebase blindly.

### 2. GitHub Git Data API / multi-file commit path

If a direct workspace is unavailable but Git Data operations are supported, construct one commit containing the complete file set rather than repeatedly using per-file contents endpoints that each auto-commit:

1. Read the selected base branch's current commit and tree.
2. Prepare the final file contents and create blobs for changed/new files. Represent removals as tree deletions using the API's documented semantics.
3. Build one tree based on the selected base tree, including all approved changed paths in one operation.
4. Create one commit with the new tree and the selected base commit as its parent.
5. Move the Syra-owned task branch reference to this commit only after validating the intended tree and confirming the reference has not moved unexpectedly. Use a compare-and-swap/expected-head check where supported.
6. Verify the branch now contains one task commit relative to the selected base before creating the initial PR.

Use the current official GitHub API docs and the actual connected tool schemas. Do not invent argument names, assume binary content support, or assume a tool can perform a write just because its read counterpart exists.

Official GitHub API references:

- Create a blob: https://docs.github.com/en/rest/git/blobs#create-a-blob
- Create a tree: https://docs.github.com/en/rest/git/trees#create-a-tree
- Create a commit: https://docs.github.com/en/rest/git/commits#create-a-commit
- Update a reference: https://docs.github.com/en/rest/git/refs#update-a-reference

### 3. Connector auto-commits each file

If a contents API creates a commit per file:

1. Finish the approved implementation and perform every supported check before PR creation.
2. Inspect the full commit range against the selected base, including commit authors/messages and the complete final diff.
3. Confirm every commit in the range belongs to this Syra task. If the range contains unrelated or user-authored commits, stop and ask how to proceed; do not squash them into the task commit.
4. Use a supported local Git or Git Data API path to replace the task-only history with a single commit whose parent is the selected base tip and whose tree represents the verified final task state.
5. Update the Syra-owned task branch safely, using an expected-head/lease check when available. Never force-update a shared or user-owned branch, or overwrite a ref that changed after inspection.
6. Re-fetch/inspect the branch head and compare it with the base. Confirm that the tree has the intended changes and exactly one task commit remains.

If safe history rewriting is unsupported before the first PR is created, do not silently open a multi-commit PR while claiming it was squashed. Report the connector limitation and offer a supported workspace, a new clean task branch, or a user-approved alternative. If a PR already exists, treat new work as a normal PR update: add commit(s) without re-squashing or rewriting history just to regain the one-commit shape. Inspect the changed diff and check results. Only squash/rebase an existing PR branch when the user explicitly requests it or a distinct safety issue justifies it and the rewrite is authorized.

## Safety checks before rewriting history

Never squash until all are true:

- The task branch is known and owned by Syra/the current task, or the user explicitly authorized rewriting that branch.
- The selected base branch and its current commit SHA are known.
- The range of commits to be condensed has been inspected.
- The range contains no unrelated work that would be lost or wrongly attributed.
- The complete final diff contains only approved task changes; unrelated pre-existing work remains untouched.
- The task has been verified as far as the environment permits.
- The branch head has not changed since the last inspection; use expected-SHA/lease protection if supported.

If the base advanced since task creation, compare the latest base and task branch. Update/rebase only using a safe supported workflow; handle conflicts deliberately. If the base and task branch diverged in a way the tools cannot resolve safely, pause and report the conflict.

## Initial PR creation gate

The one-task-commit invariant applies immediately before the PR is first created, not to every subsequent update. Create the initial PR only after:

1. The approved scope is complete.
2. The final diff is reviewed.
3. Relevant checks have known outcomes.
4. The branch has exactly one task commit relative to the selected base, or the inability to meet this invariant has been surfaced and the user has explicitly approved a different policy.
5. The branch and base names are verified.

The initial PR should include the problem, solution, key files/areas, tests/checks and their statuses, relevant migration/database notes, and known limitations. Use the repository's PR template when available. Verify the resulting PR URL, number, head branch, and base branch. Never merge the PR by default.

## Updating an existing PR

For a follow-up update to an already-open PR:

- Review the current branch and diff, and run supported checks for the new changes.
- Add one or more normal task commits as appropriate; preserving reviewable history is preferred over rewriting it.
- Do **not** squash or rebase solely to restore exactly one task commit relative to the base. The one-commit invariant is a gate for initial PR creation only.
- Squash/rebase an existing PR branch only when the user explicitly requests that operation or a distinct safety issue warrants it and the history rewrite is explicitly authorized. Inspect the complete commit range and use an expected-head/lease check where supported.
