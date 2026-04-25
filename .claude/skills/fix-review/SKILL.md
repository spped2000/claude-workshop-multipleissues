---
name: fix-review
description: Read PR review comments via gh CLI and fix all requested changes. Use when a PR has review feedback to address.
argument-hint: [pr-number]
disable-model-invocation: true
allowed-tools: Read Write Edit Bash Glob Grep
---

# Fix Review Comments for PR #$0

## Step 1 — Read Review Comments

This project is fork-based — PRs live on the upstream repo. Detect upstream:
- !`git remote get-url upstream 2>/dev/null | sed -E 's#.*github.com[:/](.+/.+)\.git#\1#' || echo "spped2000/claude-workshop-multipleissues"`

Read review comments on PR #$0:

```bash
gh pr view $0 --repo <upstream> --comments
gh api repos/<upstream>/pulls/$0/comments
```

List each comment with: file path, line number, and what the reviewer requested.

## Step 2 — Fix

Fix each comment exactly as requested.
Do not refactor or change code that was not mentioned in the review.
Do not add new features or tests beyond what reviewers explicitly asked for.
If a comment is unclear, make the minimal reasonable interpretation.

## Step 3 — Verify

Run the full test suite — all tests must pass.
List every file changed and which review comment it addresses.

## Step 4 — Push

Commit with message: `fix: address PR #$0 review comments`
Push to the existing branch on **your fork** (`origin`) — do not open a new PR.
The cross-fork PR will pick up the new commit automatically.
Report the commit SHA when done.
