# Continue.dev Adapter

[Continue.dev](https://continue.dev) is an open-source AI coding assistant that runs inside VS Code and JetBrains IDEs. It connects to multiple AI providers (Claude, GPT-4, Gemini, local models via Ollama) and is fully self-hosted.

---

## What Continue Reads Automatically

| File | Auto-read | Notes |
|---|---|---|
| `.continue/config.json` | ✅ Yes | Tool configuration (models, context providers, slash commands) |
| `CLAUDE.md` | ❌ No | Must be added as a context provider or referenced manually |
| `memory/MEMORY.md` | ❌ No | Must be referenced with `@file` or added as a custom context provider |

Continue does not have a built-in "rules" file like Claude Code or Windsurf. Context is controlled through config and manual `@file` references.

---

## Setup

### Step 1: Create your context files

Keep `CLAUDE.md` and `memory/MEMORY.md` in your project root as described in the main framework. Continue will reference them via `@file`.

### Step 2: Create a custom slash command for session start

In `.continue/config.json`, add a slash command that loads your memory context:

```json
{
  "slashCommands": [
    {
      "name": "continue",
      "description": "Resume from last session — reads CLAUDE.md and MEMORY.md",
      "prompt": "Read @file ./CLAUDE.md and @file ./memory/MEMORY.md. Then resume from where we left off based on the session notes in MEMORY.md."
    }
  ]
}
```

Now type `/continue` at the start of each session to load your project context.

### Step 3: Add a custom system prompt

In `.continue/config.json`, add a system message that applies to all sessions:

```json
{
  "models": [
    {
      "title": "Claude Sonnet",
      "provider": "anthropic",
      "model": "claude-sonnet-4-5",
      "systemMessage": "You are working on a software project. At the start of each session, read CLAUDE.md and memory/MEMORY.md if they exist. Follow the rules in CLAUDE.md. Update memory/MEMORY.md after significant work. Archive files instead of deleting. Stay within the project folder."
    }
  ]
}
```

### Step 4: Reference files inline

During any session, use Continue's `@file` syntax to add context:

```
@file ./CLAUDE.md
@file ./memory/MEMORY.md
What's the current state of the project?
```

Or after a correction, use:
```
@file ./memory/MEMORY.md
Add this to Locked Preferences: [correction]. Update the file.
```

---

## Custom Context Providers

For teams or power users, Continue supports custom context providers that can automatically inject your CLAUDE.md and MEMORY.md into every conversation. Create a simple provider in `.continue/config.json`:

```json
{
  "contextProviders": [
    {
      "name": "file",
      "params": {}
    },
    {
      "name": "codebase",
      "params": {}
    }
  ]
}
```

Then type `@CLAUDE.md` or `@MEMORY.md` in chat — Continue will autocomplete the file reference.

---

## Starting a Session

Option A (slash command, recommended):
```
/continue
```

Option B (manual):
```
Read @file ./CLAUDE.md and @file ./memory/MEMORY.md, then continue from where we left off.
```

Option C (if using Ollama for privacy):
Same approach works — the slash command and file references are model-agnostic.

---

## Continue-Specific Features

**Multiple providers:** Continue lets you switch between Claude, GPT-4, Gemini, and local models in the same conversation. Your CLAUDE.md rules apply regardless of which model you're using.

**Codebase indexing:** Continue indexes your codebase for semantic search. Combined with the Project Map in MEMORY.md, AI can navigate your project efficiently without reading every file.

**Local models (Ollama):** For sensitive projects, Continue + Ollama runs everything locally. The memory-first pattern works the same — `@file` references pull from your local files, nothing leaves your machine.

**JetBrains support:** Continue works in IntelliJ, PyCharm, WebStorm, etc. Same setup applies — `.continue/config.json` controls behavior.

---

## Limitations

- **No automatic memory reading** — Continue doesn't read any file automatically. `/continue` slash command or manual `@file` is required every session.
- **No global rules file** — Rules must be set per-model in `config.json` system message, or referenced via `@file` each session.
- **Learning loop is manual** — When you correct AI, you must explicitly tell it to update MEMORY.md. It won't do it autonomously.

---

## Recommended File Structure

```
project/
├── .continue/
│   └── config.json        ← Model config, slash commands, system prompt
├── CLAUDE.md              ← Project rules (reference with @file or /continue)
├── memory/
│   └── MEMORY.md          ← Active context (reference with @file or /continue)
└── [project files]
```

---

## See Also

- [adapters/claude-code.md](claude-code.md) — native support, most automated
- [adapters/cursor.md](cursor.md) — `.cursorrules` equivalent
- [patterns/session-continuity.md](../patterns/session-continuity.md) — the two-file system
- [Continue.dev docs](https://docs.continue.dev) — official configuration reference
