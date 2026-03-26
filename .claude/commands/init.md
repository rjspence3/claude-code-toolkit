---
description: "Initialize a new project with standard structure, Claude Code config, and git. Usage: /init <project-name> [type]"
---

Set up a new project. Follow every step below in order.

**Input:** $ARGUMENTS

---

## Step 1: Parse Arguments

Extract from the input:
- **Project name** — the directory name (use the exact casing provided)
- **Project type** — if specified as a second argument

If the project type isn't specified or clear from context, ask the user to choose:

| Type | Description |
|------|-------------|
| `python-tool` | Python library/script, no web UI |
| `fastapi` | FastAPI backend |
| `nextjs` | Next.js frontend (App Router, TypeScript, Tailwind) |
| `fullstack` | Next.js frontend + FastAPI backend |
| `vite` | Vite + React + TypeScript frontend |

---

## Step 2: Create Project Directory

```bash
mkdir -p <project-name>
```

If the directory already exists and has files, STOP and tell the user. Don't overwrite.

---

## Step 3: Project Scaffolding

Create type-specific project structure:

### python-tool
```
<project>/
├── src/<package_name>/
│   ├── __init__.py
│   └── main.py
├── tests/
│   └── __init__.py
├── pyproject.toml          # with [project.optional-dependencies] dev = ["pytest", "ruff"]
└── README.md
```
Then: `cd <project> && python3 -m venv .venv`

### fastapi
```
<project>/
├── src/<package_name>/
│   ├── __init__.py
│   └── main.py             # FastAPI app with health endpoint
├── tests/
│   └── __init__.py
├── pyproject.toml           # include fastapi, uvicorn
└── README.md
```
Then: `cd <project> && python3 -m venv .venv`

### nextjs
```
<project>/
└── (created by npx create-next-app)
```
Run: `npx create-next-app@latest <project> --typescript --tailwind --eslint --app --src-dir --no-import-alias`

### fullstack
```
<project>/
├── backend/
│   ├── src/<package_name>/
│   │   ├── __init__.py
│   │   └── main.py
│   ├── tests/
│   │   └── __init__.py
│   └── pyproject.toml
├── frontend/
│   └── (created by npx create-next-app)
└── README.md
```
Then: `cd <project>/backend && python3 -m venv .venv`
Then: `cd <project>/frontend && npx create-next-app@latest . --typescript --tailwind --eslint --app --src-dir --no-import-alias`

### vite
```
<project>/
└── (created by npm create vite)
```
Run: `npm create vite@latest <project> -- --template react-ts`

---

## Step 4: Claude Code Setup

Create a CLAUDE.md with project-specific context:

```markdown
# <Project Name>

## What this is
<one-line description — fill in or ask the user>

## Stack
<language, framework, key dependencies>

## Development
<how to run, test, build>

## Conventions
<any project-specific patterns — fill in as they emerge>
```

Create `.claudeignore` if the project has directories Claude shouldn't read:
```
node_modules/
.venv/
dist/
.next/
__pycache__/
```

---

## Step 5: Initialize Git

```bash
cd <project>
git init
git add .
git commit -m "Initial project scaffold"
```

---

## Step 6: Summary

Show the user:

1. **Project created:** path to project
2. **Type:** <type>
3. **Files created:** list all files
4. **Next steps:**
   - `cd <project>`
   - Activate venv: `source .venv/bin/activate` (if Python)
   - Install deps: `pip install -e ".[dev]"` or `npm install`
   - Start coding

---

## Rules

- Do NOT overwrite existing files or directories without asking.
- Use the exact project name casing the user provides for the directory.
- If anything goes wrong, stop and explain rather than continuing with a broken state.
