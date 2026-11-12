# Windsurf Adapter

[Windsurf](https://codeium.com/windsurf) is an AI-native IDE from Codeium with an agentic AI called Cascade. It supports multi-file awareness, autonomous task execution, and context rules via a global rules file.

---

## What Windsurf Reads Automatically

| File | Auto-read | Notes |
|---|---|---|
| `.windsurfrules` (project root) | ✅ Yes | Read every session automatically |
| `~/.codeium/windsurf/memories/global_rules.md` | ✅ Yes | Global rules across all projects |
| `memory/MEMORY.md` | ❌ No | Must be referenced manually or via rules |
| `CLAUDE.md` | ❌ No | Rename to `.windsurfrules` or reference manually |

---

## Setup

### Step 1: Create `.windsurfrules`

Copy the CLAUDE.md template to `.windsurfrules` in your project root:

```bash
cp templates/CLAUDE.md.template .windsurfrules
```

Fill in the template. Windsurf reads this file at the start of every Cascade session.

### Step 2: Add memory reference to your rules

In `.windsurfrules`, add a session protocol that tells Cascade to read the memory file:

```markdown
## Session Protocol
1. Read `memory/MEMORY.md` for current project context
2. Check existing docs before creating new files
3. Update `memory/MEMORY.md` after significant work
```

Cascade follows explicit instructions in `.windsurfrules`, so this creates the memory-first behavior.

### Step 3: Optional — global rules

For preferences that apply across all projects (coding style, safety rules), add them to:
`~/.codeium/windsurf/memories/global_rules.md`

```markdown
# Global Rules

- Archive files, don't delete. Use -..delete suffix for uncertain removals.
- Ask before running destructive operations (drops, force pushes, resets)
- Stay in project folder unless explicitly told to leave
- Update project MEMORY.md at end of every session
```

---

## Starting a Session

Unlike Claude Code, Windsurf does not have a built-in "Continue" command. Use this approach:

At the start of each session, type:
> "Read `.windsurfrules` and `memory/MEMORY.md`, then continue from where we left off."

Or add a shorthand note at the top of your `.windsurfrules`:
```markdown
## Activation
When user says "Continue", read memory/MEMORY.md and resume previous work.
```

---

## Windsurf-Specific Features

**Cascade flows:** Windsurf's Cascade can plan and execute multi-step tasks autonomously. Pair this with the [Error Recovery](../patterns/error-recovery.md) pattern — always commit before letting Cascade run large autonomous operations.

**Inline suggestions:** Cascade also powers inline completions (like Copilot). Your `.windsurfrules` doesn't affect inline completions — only Cascade chat sessions.

**File context:** Cascade has good codebase awareness built-in. You don't need to manually reference files as often as with Copilot — it can find things. But it still benefits from the project map in MEMORY.md for large codebases.

---

## Limitations

- **No automatic memory updates** — Cascade won't update MEMORY.md unless you tell it to. Add "Update memory/MEMORY.md at end of session" to your rules.
- **Global rules file path may change** — Codeium updates Windsurf frequently. Check `~/.codeium/windsurf/` if the path above doesn't exist.
- **Cascade sessions don't share context** — like all tools, each Cascade session starts fresh. `.windsurfrules` + MEMORY.md carry the context.

---

## Recommended `.windsurfrules` Structure

```markdown
# [PROJECT NAME] — Windsurf Rules

## Project Overview
Name: [PROJECT]
Stack: [STACK]
Status: [STATUS]

## Session Protocol
1. Read memory/MEMORY.md for current context
2. Check existing files before creating new ones
3. Update memory/MEMORY.md after significant work
4. When user says "Continue", resume from last session state in MEMORY.md

## Working Preferences
- Archive files instead of deleting
- Use -..delete suffix for uncertain removals
- Ask approval before: deleting files, force pushes, schema migrations
- Stay within project folder

## Code Conventions
[Your project-specific rules]

## Tools Available
[Scripts and tools in this project]

Last Updated: [DATE]
```

---

## See Also

- [adapters/claude-code.md](claude-code.md) — native support, most automated
- [adapters/cursor.md](cursor.md) — `.cursorrules` equivalent
- [patterns/session-continuity.md](../patterns/session-continuity.md) — the two-file system
