# Claude Code Toolkit

> My personal Claude Code commands and agents after 6 months of daily use. Steal freely.

## What's in here

12 slash commands and 3 agent definitions I actually use. Not a framework — just the stuff that survived.

### Commands that matter most

**`/build`** — Spawns planner → coder → reviewer pipeline. Give it a task, come back to working code.

**`/dispatch`** — Breaks a large task into parallel subtasks and runs them. Works for refactors, audits, migrations.

**`/security-audit`** — Full security pass: hardcoded secrets, SQL injection, auth gaps, dependency vulns. Produces a prioritized findings report.

### All commands

| Command | What it does |
|---------|-------------|
| `/build` | Multi-agent build pipeline (planner → coder → reviewer) |
| `/dispatch` | Parallel task execution |
| `/security-audit` | Security scan with prioritized findings |
| `/beta-test` | Personality-driven QA via multiple agents |
| `/site-audit` | Accessibility + SEO + GEO audit with letter grade |
| `/handoff` | Generate a handoff doc from current session state |
| `/fresh` | Clear context, start clean without losing work |
| `/init` | Initialize a new project with standard structure |
| `/plan` | Generate a phased implementation plan |
| `/railway-preflight` | Pre-deploy checklist for Railway |
| `/verify` | Run verification pass on completed work |
| `/wait-for` | Poll until a condition is met |

### Agents

| Agent | Role |
|-------|------|
| `planner.md` | Breaks tasks into phases, writes PLAN.md |
| `coder.md` | Implements phases, commits after each |
| `reviewer.md` | Reviews output, flags issues, suggests improvements |

## Install

```bash
# Clone into your Claude config directory
git clone https://github.com/rjspence3/claude-code-toolkit ~/.claude/toolkit

# Copy commands and agents
cp -r ~/.claude/toolkit/.claude/commands/* ~/.claude/commands/
cp -r ~/.claude/toolkit/.claude/agents/* ~/.claude/agents/
```

That's it. Commands are available immediately in Claude Code as `/command-name`.

## Usage

```bash
# Build something
/build implement a rate limiter for the API

# Run a security audit
/security-audit

# Parallel refactor
/dispatch refactor all API routes to use the new error handler
```

## What I've learned

After 6 months: the commands that actually stick are the ones that handle the annoying parts of a workflow, not the parts that are interesting. Nobody needs a command that makes coding more exciting. You need commands that handle the handoff, the cleanup, the review pass — the parts you'd otherwise skip.

## Background

I wrote about the experiment that shaped these in [this post](https://substack.com/@rjspence3). The prompt format doesn't matter as much as you think. The workflow does.

---

Built by [Rob Spence](https://nomouthlabs.com). MIT License.
