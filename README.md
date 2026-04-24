# Workshop API — Multi-Issue Workshop (gh CLI edition)

Base FastAPI project for the **Multi-Issue Workshop** — GitHub Issue → PR workflow,
powered by Claude Code + the `gh` CLI (no MCP, no shared tokens).

## Quick Start

```bash
# 1. Authenticate with your own GitHub account (one-time, browser-based)
gh auth login

# 2. Install dependencies
uv sync

# 3. Run dev server
uv run uvicorn app.main:app --reload
```

API: http://localhost:8000
Docs: http://localhost:8000/docs

## Run Tests

```bash
uv run pytest
```

## Project Structure

```
app/
  main.py          — FastAPI app entry point
  models.py        — Pydantic request/response models
  database.py      — In-memory storage + helper functions
  routers/
    users.py       — User CRUD routes
tests/
  conftest.py      — Test fixtures (auto-resets DB between tests)
  test_users.py    — CRUD tests
```

## Available Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | /users | List all users |
| POST | /users | Create a user |
| GET | /users/{id} | Get a user |
| PUT | /users/{id} | Update a user |
| DELETE | /users/{id} | Delete a user |

## Your Task

1. Pick up your assigned issue with `gh issue view <N>`
2. Implement the feature following the acceptance criteria
3. Write tests in a separate file `tests/test_<feature>.py`
4. Create a PR referencing the issue number

Example prompt to give Claude:

```
Read issue #<your group's issue number> using `gh issue view`.
Implement the feature, write tests, and create a PR.
```

See [WORKSHOP.md](../WORKSHOP.md) for the full step-by-step guide.
