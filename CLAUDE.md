# Workshop API — Project Context for Claude Code

## Project Overview

FastAPI REST API with in-memory user storage. Python 3.11+.

The API is **intentionally incomplete** — your assigned GitHub issue describes exactly what to add.

## GitHub Workflow — use `gh` CLI (fork-based)

This project uses the **GitHub CLI (`gh`)** instead of the GitHub MCP server.
Each participant works on **their own fork** of `spped2000/claude-workshop-multipleissues`
and opens cross-fork PRs back to upstream. No collaborator access required.

- `origin` → participant's fork (push commits here)
- `upstream` → `spped2000/claude-workshop-multipleissues` (issues + PR target live here)

### Common commands Claude can run

```bash
# Read assigned issue (issues live on UPSTREAM, not the fork)
gh issue view <N> --repo spped2000/claude-workshop-multipleissues --json title,body,labels

# List open issues on upstream
gh issue list --repo spped2000/claude-workshop-multipleissues --state open

# Push to your fork
git push -u origin <branch>

# Open a PR targeting UPSTREAM (always pass --repo to be safe)
gh pr create \
  --repo spped2000/claude-workshop-multipleissues \
  --title "..." \
  --body "Closes #<N>"

# Read review comments on a PR
gh pr view <N> --repo spped2000/claude-workshop-multipleissues --comments
```

`gh` reads auth from the OS keyring — no token env vars needed.

## Your Task

1. Read your assigned issue with `gh issue view <N> --repo spped2000/claude-workshop-multipleissues`
2. Implement the feature described in the acceptance criteria
3. Write or update tests — all existing tests must continue to pass
4. Push to your fork and open a PR back to upstream referencing the issue number (e.g. "Closes #1")

## Coding Conventions

- Use `async def` for all route handlers and test functions
- Pydantic v2 models live in `app/models.py`
- In-memory storage only — no database imports, no SQLAlchemy
- Add new routes to `app/routers/users.py` or directly to `app/main.py` (check your issue)
- Tests go in `tests/`, use `pytest-asyncio` with `httpx.AsyncClient`
- Add new dependencies with `uv add <package>` only if your issue explicitly requires one

## Project Structure

```
app/main.py           — FastAPI app, middleware registration, router mount
app/models.py         — All Pydantic request/response models
app/database.py       — In-memory dict storage + helper functions
app/routers/users.py  — User CRUD routes
tests/conftest.py     — Shared fixtures (auto-resets DB between tests)
tests/test_users.py   — Existing CRUD tests
```

## Run Commands

```bash
uv run pytest          # run tests
uv run pytest -v       # verbose
uv run uvicorn app.main:app --reload  # dev server
uv add <package>       # add new dependency
```

## Test Pattern

```python
async def test_example(client):  # client fixture from conftest.py
    response = await client.get("/some-endpoint")
    assert response.status_code == 200
```
