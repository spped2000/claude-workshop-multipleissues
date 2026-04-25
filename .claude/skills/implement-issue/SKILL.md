---
name: implement-issue
description: Read a GitHub issue via gh CLI, implement the feature, write tests in a separate file, and open a PR. Use when given an issue number to work on.
argument-hint: [issue-number]
disable-model-invocation: true
allowed-tools: Read Write Edit Glob Grep Bash
---

# Implement Issue #$0

## Step 1 — Read the Issue via gh CLI

Detect the upstream repo:
- !`git remote get-url upstream 2>/dev/null | sed -E 's#.*github.com[:/](.+/.+)\.git#\1#' || echo "spped2000/claude-workshop-multipleissues"`

Run `gh issue view $0 --repo <upstream> --json title,body,labels` to fetch the issue.
Display the acceptance criteria as a numbered checklist before proceeding.
If `gh` is not authenticated, ask the user to run `gh auth login` first.

## Step 2 — Implement

Read CLAUDE.md first to understand the project structure and conventions.
Then implement all acceptance criteria:

- Use `async def` for all route handlers
- Edit the correct file as described in the project structure (main.py or routers/users.py)
- Do not modify logic unrelated to this issue
- If a new dependency is required, run `uv add <package>` and commit pyproject.toml

## Step 3 — Write Tests

Create a new file `tests/test_<feature_name>.py` — do not modify test_users.py.
Write tests covering every acceptance criteria in the issue.
Run `uv run pytest -v` — all tests including the original 9 must pass.
Fix any failures before moving to the next step.

## Step 4 — Create a Cross-Fork PR via gh CLI

This project is fork-based. `origin` is the participant's fork; `upstream` is the
target repo for the PR.

- Create branch `feat/issue-$0-<short-description>` before committing
- Stage and commit the changes (conventional commit format)
- Push the branch to the fork: `git push -u origin <branch-name>`
- Open the PR targeting upstream:
  ```bash
  gh pr create \
    --repo <upstream-detected-in-step-1> \
    --title "<issue title>" \
    --body "Closes #$0

  <short description of what was implemented>"
  ```

Display the PR URL when complete.
