---
name: explore-issue
description: Research a GitHub issue and summarize findings without polluting main context. Use when you want to understand an issue before implementing.
argument-hint: [issue-number]
context: fork
agent: Explore
allowed-tools: Read Glob Grep Bash
---

# Explore Issue #$0

## Step 1 — Detect Upstream Repository

This project is fork-based — issues live on `upstream`, not `origin`.

- !`git remote get-url upstream 2>/dev/null | sed -E 's#.*github.com[:/](.+/.+)\.git#\1#' || echo "spped2000/claude-workshop-multipleissues"`

## Step 2 — Read the Issue

Run `gh issue view $0 --repo <upstream-detected-above> --json title,body,labels`
to fetch issue $0.
If `gh` is not authenticated, ask the user to run `gh auth login` first.

## Step 3 — Analyze Codebase Impact

Find all files that need to change to implement issue #$0:
- Search for relevant functions, models, routes, and tests
- Check CLAUDE.md for project structure and conventions
- Identify dependencies that may need to be added

## Step 4 — Return Summary

Return a concise report:

```
Issue #$0: <title>

Files to modify:
  - <file path> → <what needs to change and why>
  - ...

New files to create:
  - <file path> → <purpose>

Dependencies to add:
  - <package> (if any)

Key considerations:
  - <gotchas, ordering issues, breaking changes>
```

Do NOT make any edits. This is a read-only exploration.
