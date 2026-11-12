# Quick Start — 5 Minutes to Better AI Collaboration

## Step 1: Copy CLAUDE.md (1 min)

Copy `templates/CLAUDE.md.template` to your project root:

```bash
cp templates/CLAUDE.md.template /path/to/your/project/CLAUDE.md
```

Edit the template:
- Replace `[PROJECT NAME]` with your project name
- Adjust rules to fit your workflow
- Keep it under 100 lines

## Step 2: Create Memory Folder (30 sec)

```bash
cd /path/to/your/project
mkdir -p memory
```

The AI will create `memory/MEMORY.md` automatically as you work.

## Step 3: Start Chatting (immediate)

Open your AI tool (Claude Code, Copilot, Cursor) and start working.

The AI will:
- Read `CLAUDE.md` for project rules
- Build `memory/MEMORY.md` as context accumulates
- Update memory when you correct mistakes

## Step 4: The Magic — Corrections Become Rules

When AI makes a mistake:

1. **Correct it:** "No, don't use emojis as icons. Use SVG."
2. **AI updates memory:** Adds this to preferences
3. **Never repeats:** Next session, AI knows

This is the learning loop. The more you work together, the smarter it gets about YOUR project.

---

## What You'll Have After a Few Sessions

```
your-project/
├── CLAUDE.md                    ← Your rules (you wrote once)
├── memory/
│   ├── MEMORY.md                ← Project context (AI maintains)
│   ├── debugging.md             ← Detailed notes (if needed)
│   └── decisions.md             ← Architectural choices (if needed)
└── [your project files]
```

---

## Tips

**Keep MEMORY.md lean:**
- Index only, under 200 lines
- Link to topic files for details

**Keep CLAUDE.md focused:**
- Rules, not explanations
- Under 100 lines
- Update rarely (when workflow changes)

**Use "Continue":**
- Start new sessions with "Continue" to resume context
- AI reads memory and picks up where you left off

---

## Next Steps

- Read `patterns/` to understand WHY this works
- Check `adapters/` for platform-specific tips
- See `examples/` for real implementations

---

That's it. No complex setup. No dependencies. Just two files and a folder.
