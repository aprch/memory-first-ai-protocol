# Adapter: Cursor

Cursor has good support for custom instructions through `.cursorrules` files.

## How It Works

Cursor reads `.cursorrules` from your project root automatically. This is similar to CLAUDE.md for Claude Code.

## Setup

### 1. Create .cursorrules

```bash
cp templates/CLAUDE.md.template .cursorrules
```

Or create fresh:
```bash
touch .cursorrules
```

### 2. Add Your Rules

`.cursorrules` uses the same format as CLAUDE.md:

```markdown
# Project Rules

## Project Overview
Name: My Project
Stack: React + Node.js

## Coding Conventions
- Use TypeScript strict mode
- Prefer functional components
- No emojis as icons

## Session Protocol
1. Check existing code before writing new
2. Follow project conventions
3. Ask before destructive changes
```

### 3. Memory Handling

Cursor doesn't have auto-memory like Claude Code. Create manually:

```bash
mkdir -p memory
touch memory/MEMORY.md
```

Reference it in `.cursorrules`:
```markdown
## Context
Read memory/MEMORY.md for current project context before starting work.
```

## Cursor-Specific Features

### .cursorrules
- Automatically read for every chat
- Project-specific rules
- Placed in project root

### @codebase
- Use `@codebase` to give Cursor full project context
- Useful for "find where X is used" questions

### @file
- Reference specific files: `@file:memory/MEMORY.md`
- Include context in your questions

### Composer
- Multi-file edits
- Use for larger changes
- Still follows .cursorrules

## Adapting the Framework

### CLAUDE.md → .cursorrules

The format is essentially the same. Just rename:

```bash
cp templates/CLAUDE.md.template .cursorrules
```

### Memory → Semi-Manual

Include memory reference in .cursorrules:
```markdown
## Session Start
Read @file:memory/MEMORY.md for current context.
```

Then manually update MEMORY.md after sessions.

### Learning Loops → Mostly Automatic

When corrected:
1. Tell Cursor to remember
2. Either it updates its session context
3. Or manually add to .cursorrules

For persistent rules, add to .cursorrules manually.

## Workflow

```
1. Start chat → Cursor reads .cursorrules
2. Include @file:memory/MEMORY.md if needed
3. Work on task
4. End session → Update memory/MEMORY.md
5. If new rule learned → Add to .cursorrules
```

## Tips

**For Better Context:**
```
@codebase @file:memory/MEMORY.md

[Your question here]
```

**For Consistent Style:**
- Be specific in .cursorrules
- Include examples of preferred patterns
- Mention what NOT to do

**For Memory:**
- Keep MEMORY.md open in a tab
- Cursor uses open files for context
- Reference with @file: when needed

## Example .cursorrules

```markdown
# Project: My App

## Stack
React 18, TypeScript, Tailwind CSS, Node.js

## Conventions
- Functional components only
- Use hooks, no class components
- Tailwind for styling, no CSS files
- Error boundaries around async components

## File Structure
src/
├── components/    # React components
├── hooks/         # Custom hooks
├── api/           # API calls
└── utils/         # Helpers

## Context
Read @file:memory/MEMORY.md for current project state.

## Rules
- No console.log in production code
- All functions need error handling
- Tests required for new features
```

## Limitations

- Memory not fully automatic (but .cursorrules helps)
- Learning loops need manual rule addition
- Session context doesn't persist across Cursor restarts

Better than Copilot, not quite Claude Code. But the framework applies well.
