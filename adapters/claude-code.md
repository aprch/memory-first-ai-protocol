# Adapter: Claude Code

Claude Code has **native support** for this framework. It's the easiest platform to use.

## How It Works

Claude Code automatically reads:
- `CLAUDE.md` in your project root
- `memory/MEMORY.md` (when using auto-memory feature)

No additional setup needed.

## Setup

### 1. Copy CLAUDE.md Template

```bash
cp templates/CLAUDE.md.template /path/to/your/project/CLAUDE.md
```

### 2. Enable Auto-Memory (if not already)

Claude Code creates a memory directory at:
```
~/.claude/projects/[project-path]/memory/MEMORY.md
```

Or create one in your project:
```bash
mkdir -p memory
```

### 3. Start Working

That's it. Claude Code reads CLAUDE.md automatically on every session.

## Claude Code-Specific Features

### Project-Level CLAUDE.md

Place in project root. Read every session.

### User-Level CLAUDE.md

Place at `~/.claude/CLAUDE.md` for rules that apply to ALL projects.

### Memory Persistence

Claude Code's auto-memory persists across sessions automatically. Use it.

### Session Continuity

"Continue" works naturally — Claude reads memory and resumes.

## Tips for Claude Code

### Use the Built-In Tools

Claude Code has dedicated tools. Use them instead of bash when possible:
- `Read` instead of `cat`
- `Edit` instead of `sed`
- `Grep` instead of `grep`
- `Glob` instead of `find`

### Token Efficiency

Claude Code compresses old messages automatically, but still:
- Keep MEMORY.md lean
- Don't re-read files unnecessarily
- Use bash for bulk operations

### File Safety

Claude Code asks permission for sensitive operations. But still:
- Keep archive/ pattern
- Use `-..delete` suffix for uncertain deletions
- Review changes before committing

## Example CLAUDE.md for Claude Code

See `templates/CLAUDE.md.template` — it's designed for Claude Code.

## Troubleshooting

**CLAUDE.md not being read:**
- Must be in project root (where you run claude)
- Check file name is exactly `CLAUDE.md`

**Memory not persisting:**
- Check `~/.claude/projects/` for auto-memory
- Or create `memory/MEMORY.md` in project

**Context getting lost:**
- Keep MEMORY.md under 200 lines
- Use topic files for details
