# Aider Adapter

[Aider](https://aider.chat) is a CLI-based AI coding assistant that works directly in your terminal. It has native support for persistent instructions files and automatically reads certain files — making it one of the better fits for the Memory-First protocol.

---

## What Aider Reads Automatically

| File | Auto-read | Notes |
|---|---|---|
| `.aider.conf.yml` | ✅ Yes | Configuration file (model, settings) |
| `.aider.system.md` (or `--system-prompt`) | ✅ Yes | System prompt injected every session |
| `CONVENTIONS.md` | ✅ Yes | Aider reads this automatically if present |
| `CLAUDE.md` | ⚠️ Partial | Can configure via `--read CLAUDE.md` flag |
| `memory/MEMORY.md` | ❌ No | Must be added to chat or configured as `--read` |

---

## Setup

### Step 1: Configure auto-read in `.aider.conf.yml`

Create `.aider.conf.yml` in your project root:

```yaml
# .aider.conf.yml
model: claude-sonnet-4-5       # or gpt-4o, gemini/gemini-pro, etc.
read:
  - CLAUDE.md                  # reads as system context every session
  - memory/MEMORY.md           # reads as system context every session
auto-commits: true             # auto-commit after each change (recommended)
dirty-commits: true            # allow commits even with uncommitted changes
```

With this config, Aider automatically loads both files every session — closest to Claude Code's native behavior.

### Step 2: Use CLAUDE.md directly (or rename)

Aider can read `CLAUDE.md` via the `--read` flag or via `.aider.conf.yml`. No need to rename or copy — reference it directly.

Alternatively, Aider has first-class support for `CONVENTIONS.md` — if you prefer that name, copy your CLAUDE.md content there:

```bash
cp CLAUDE.md CONVENTIONS.md
```

Aider reads `CONVENTIONS.md` automatically without any configuration.

### Step 3: Start sessions with context

With `.aider.conf.yml` configured:
```bash
aider
```
Both CLAUDE.md and MEMORY.md are pre-loaded. Tell Aider:
> "Continue from where we left off."

Without configuration:
```bash
aider --read CLAUDE.md --read memory/MEMORY.md
```

---

## The Aider Workflow

Aider works differently from chat-based tools: you tell it what to do, it edits files, and (if enabled) commits the changes automatically.

**Typical session:**
```
> Continue from where we left off. What's the current state?
[Aider reads CLAUDE.md + MEMORY.md, summarizes]

> Add user authentication using the approach described in MEMORY.md
[Aider edits files, commits automatically]

> Update memory/MEMORY.md to reflect what was done today
[Aider updates the file]
```

**Adding files to the chat:**
```
/add src/auth/middleware.ts
/add src/api/users.ts
```

Aider only edits files that are in the chat. This is built-in scope control — it can't accidentally modify files you haven't explicitly added.

---

## Memory Updates

Aider's auto-commit means your changes are tracked automatically. But MEMORY.md still needs manual updates at session end:

```
> Update memory/MEMORY.md:
> - Last session: [DATE] — Implemented JWT auth middleware, wired to user routes
> - Active work: Update the dashboard to use authenticated routes
> - Add to Locked Preferences: Always use httpOnly cookies for token storage
```

Or add this to your CLAUDE.md:
```markdown
## Session Protocol
At end of each session: update memory/MEMORY.md with what was done and what's next.
```

---

## Aider-Specific Features

**Auto-commits:** Aider commits after each change. This is excellent for the [Error Recovery](../patterns/error-recovery.md) pattern — every change is recoverable. Use `--no-auto-commits` if you want to review before committing.

**Git blame tracking:** Aider records which changes were AI-assisted in git messages. Useful for auditing what AI did in a session.

**Linting:** Aider can run linters automatically after edits. Configure in `.aider.conf.yml`:
```yaml
lint-cmd: "npm run lint"
```

**Voice mode:** Aider supports voice input. Same context files apply.

**Local models:** Aider works with Ollama and other local model servers via `--model ollama/...`. Memory-first pattern works identically.

---

## Limitations

- **CLI only** — no GUI. If you prefer a visual IDE, use Cursor, Windsurf, or Continue instead.
- **File scope required** — Aider only edits files explicitly added with `/add`. This is actually a safety feature, but you must remember to add files before asking for edits.
- **MEMORY.md not auto-updated** — Aider won't update memory on its own. Must be an explicit instruction.

---

## Recommended `.aider.conf.yml`

```yaml
model: claude-sonnet-4-5
read:
  - CLAUDE.md
  - memory/MEMORY.md
auto-commits: true
dirty-commits: false
git: true
lint-cmd: null                 # Set to your lint command if desired
```

---

## Recommended File Structure

```
project/
├── .aider.conf.yml            ← Aider configuration
├── .aider.chat.history.md     ← Auto-generated chat log (add to .gitignore if preferred)
├── CLAUDE.md                  ← Project rules (auto-loaded via .aider.conf.yml)
├── memory/
│   └── MEMORY.md              ← Active context (auto-loaded via .aider.conf.yml)
└── [project files]
```

Add to `.gitignore` if you don't want chat history committed:
```
.aider.chat.history.md
.aider.llm.history.md
```

---

## See Also

- [adapters/claude-code.md](claude-code.md) — native support in Claude Code
- [adapters/windsurf.md](windsurf.md) — GUI IDE with `.windsurfrules`
- [patterns/error-recovery.md](../patterns/error-recovery.md) — recovery when AI breaks something
- [Aider docs](https://aider.chat/docs) — official configuration reference
