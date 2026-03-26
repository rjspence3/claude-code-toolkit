# Install

```bash
# Clone into your Claude config directory
git clone https://github.com/rjspence3/claude-code-toolkit ~/.claude/toolkit

# Copy commands and agents
cp -r ~/.claude/toolkit/.claude/commands/* ~/.claude/commands/
cp -r ~/.claude/toolkit/.claude/agents/* ~/.claude/agents/
```

Commands are available immediately in Claude Code as `/command-name`.

## Manual install

If you only want specific commands, copy them individually:

```bash
# Example: just the build pipeline
cp ~/.claude/toolkit/.claude/commands/build.md ~/.claude/commands/
cp ~/.claude/toolkit/.claude/agents/planner.md ~/.claude/agents/
cp ~/.claude/toolkit/.claude/agents/coder.md ~/.claude/agents/
cp ~/.claude/toolkit/.claude/agents/reviewer.md ~/.claude/agents/
```

## Updating

```bash
cd ~/.claude/toolkit
git pull
cp -r .claude/commands/* ~/.claude/commands/
cp -r .claude/agents/* ~/.claude/agents/
```
