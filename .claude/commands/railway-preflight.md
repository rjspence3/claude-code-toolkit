---
description: "Pre-deploy readiness check for Railway. Catches broken Dockerfiles, missing dependencies, and SQLite-incompatible migrations before they cause a failed deploy."
---

Run a pre-deploy readiness check before pushing to Railway. Catches issues that
`railway up` cannot — broken Dockerfiles, missing dependencies, and
SQLite-incompatible migrations — before they cause a failed deploy.

## What it checks

1. **Dockerfile** — Source ordering (COPY before pip install), CMD form, multi-stage issues
2. **Dependencies** — Imports in your Python code vs declared dependencies in pyproject.toml/requirements.txt
3. **Migrations** — SQLite-isms that break on PostgreSQL (PRAGMA, boolean literals, etc.)

## Steps

**1. Determine project root**

Use the current working directory unless the user specifies a path.

**2. Run the checks**

For each check category, analyze the project files directly:

### Dockerfile Check
Read the project's `Dockerfile` (if present). Check for:
- Source files are COPY'd before `pip install .` or `pip install -e .`
- CMD uses exec form `["python", "..."]` unless shell variable expansion is needed
- `.dockerignore` exists if Dockerfile exists
- Detect the build backend from `pyproject.toml` (hatchling, flit, poetry, setuptools)

### Dependency Check
Scan Python files in `src/` (or project root if no `src/`):
- Collect all `import` and `from ... import` statements
- Compare against declared dependencies in `pyproject.toml` or `requirements.txt`
- Exclude stdlib modules and relative imports
- Flag any imported-but-undeclared packages

### Migration Check
Scan any `migrations/` directories recursively:
- Flag SQLite `PRAGMA` statements
- Flag boolean integer literals (`0`/`1` instead of `false`/`true` in INSERT statements)
- Flag other SQLite-specific syntax that won't work on PostgreSQL

**3. Present results**

Show a summary table:

```
| Check          | Status |
|----------------|--------|
| Dockerfile     | PASS   |
| Dependencies   | FAIL   |
| Migrations     | WARN   |
```

Status indicators: PASS, FAIL, WARN, SKIP (if not applicable — e.g. no Dockerfile, no migrations)

Then for each check with issues, show details grouped by severity (errors first):

```
### Dependencies — 1 error

FAIL: missing_dependency (src/api/app.py)
  'psutil' is imported but not declared in pyproject.toml.
  Fix: add "psutil>=5.9.0" to pyproject.toml dependencies
```

**4. Verdict**

- Any `FAIL` → **DO NOT DEPLOY.** Fix all errors first.
- Only `WARN` → **Deploy with caution.** Warnings may not break the deploy but indicate fragile patterns.
- All `PASS`/`SKIP` → **Ready to deploy.** Proceed with `railway up`.

**5. Offer to fix errors automatically**

For each error, ask the user if they want you to apply the fix:

- `missing_dependency` → add the package to pyproject.toml
- `source_order` → reorder COPY/RUN instructions in Dockerfile
- `boolean_integer_literal` → replace 0/1 with false/true in the migration INSERT
- `pragma` → remove or replace PRAGMA with PostgreSQL equivalent
- `exec_form_with_var` → convert CMD to shell form

Apply only what the user confirms. After fixing, re-run the affected check to verify.
