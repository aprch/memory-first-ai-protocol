# Pattern: Session Continuity

## The Problem

Every new chat starts from zero. You have to:
- Explain what you're working on
- Remind AI of decisions made
- Re-establish preferences
- Rebuild context from scratch

This wastes time and tokens. And things get lost between sessions.

## The Solution

Persistent files that carry context across sessions.

### 1. The Two-File System

```
CLAUDE.md  → Rules (stable, rarely changes)
MEMORY.md  → Context (active, changes often)
```

AI reads both at session start. Instantly knows:
- How to work with you (rules)
- What you're working on (context)

### 2. The Session Protocol

Every session follows the same pattern:

```
1. Read CLAUDE.md (rules)
2. Read memory/MEMORY.md (context)
3. Work on the task
4. Update MEMORY.md with new context
5. Repeat
```

This is explicit in your CLAUDE.md so AI follows it automatically.

### 3. The "Continue" Pattern

Start sessions with "Continue" when resuming work.

AI sees this, reads memory, picks up where you left off. No explanation needed.

### 4. Phase-Based Execution

For large projects, track progress in phases:

```markdown
## Current Phase

**Phase 2:** Feature Development
- [x] User authentication
- [ ] Dashboard widgets ← Current
- [ ] API integration
```

AI knows where you are in the project without you explaining.

### 5. Session Notes

Keep brief notes from recent sessions in MEMORY.md:

```markdown
## Session Notes

**2024-01-15:** Finished auth flow, started dashboard. Next: widget API.
**2024-01-14:** Set up project structure, chose tech stack.
```

Old notes get deleted as new ones added. Keeps it fresh.

## How to Implement

Add to your CLAUDE.md:

```markdown
## Session Protocol

1. Read memory/MEMORY.md for current context
2. Check existing docs before creating new files
3. Build using project conventions
4. Update memory after significant work
```

## Measuring Success

You're doing it right if:
- "Continue" actually continues your work
- You don't explain the same things twice
- AI remembers decisions from past sessions
- New sessions feel like resuming, not restarting
