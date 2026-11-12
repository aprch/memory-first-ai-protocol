# Memory-First AI Protocol

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Works with local models](https://img.shields.io/badge/local%20models-Ollama%20%7C%20LM%20Studio%20%7C%20llama.cpp-orange.svg)](#using-it-with-local-models)

A plain-Markdown protocol that gives any AI coding assistant persistent memory, locked-in preferences, and learning loops. No dependencies, no server, no vendor lock-in. Works with hosted models (Claude, GPT, Gemini) and with **local models** running on your own machine.

**The problem:** most people use AI as if it were stateless. Every session starts from zero, the model repeats the same mistakes, and you repeat the same instructions.

**The fix:** two files and a folder. The model reads rules from `CLAUDE.md`, keeps context in `memory/MEMORY.md`, and writes your corrections back to memory so they stick.

---

## Who this is for

- **Developers** who want their assistant to remember project decisions between sessions.
- **Local model testers** who want a repeatable way to check whether a model can follow rules, keep memory, and resume work. See [Testing a model](#testing-a-model-against-the-protocol).
- **Teams** running several AI sessions on one codebase. See [multi-agent coordination](patterns/multi-agent-coordination.md).

---

## How it works

```
Read every session:
├── CLAUDE.md          ← Rules (short, <100 lines)
└── memory/MEMORY.md   ← Active context (index only, <200 lines)

Read only when needed:
└── patterns/          ← Deep dives, mostly for humans

Written by the model as you work:
└── memory/*.md        ← Topic notes linked from MEMORY.md
```

The model doesn't need to know *why* the patterns work. It only needs the rules. Humans read `patterns/` for the reasoning; the model reads `CLAUDE.md`.

---

## Quick start (5 minutes)

```bash
git clone https://github.com/aprch/memory-first-ai-protocol.git
cp memory-first-ai-protocol/templates/CLAUDE.md.template /path/to/your/project/CLAUDE.md
mkdir -p /path/to/your/project/memory
```

1. Edit `CLAUDE.md`: replace `[PROJECT NAME]`, trim rules you don't need.
2. Open your AI tool in the project and start working.
3. Correct the model when it gets something wrong. It records the correction in `memory/MEMORY.md`.
4. Next session, say **"Continue"**. It reads memory and picks up where you left off.

If your tool doesn't read `CLAUDE.md` on its own, open each session with:

> Read CLAUDE.md and memory/MEMORY.md, then continue.

Full walkthrough: [QUICK-START.md](QUICK-START.md).

---

## Using it with local models

The protocol is just files, so any model that can read and write files in your project can use it. The tool around the model matters more than the model itself.

| Tool | Local backends | Auto-loads memory? | Adapter |
|---|---|---|---|
| [Aider](https://aider.chat) | Ollama, LM Studio, any OpenAI-compatible server | ✅ via `.aider.conf.yml` `read:` | [aider.md](adapters/aider.md) |
| [Continue.dev](https://continue.dev) | Ollama, LM Studio, llama.cpp | ⚠️ via slash command / system message | [continue.md](adapters/continue.md) |
| Other agents with file access | Whatever they support | ❌ Paste the opening line above | [claude-code.md](adapters/claude-code.md) is the closest template |

**Aider + Ollama example:**

```bash
ollama pull qwen2.5-coder:14b
export OLLAMA_API_BASE=http://127.0.0.1:11434
aider --model ollama_chat/qwen2.5-coder:14b --read CLAUDE.md --read memory/MEMORY.md
```

### Tips for smaller context windows

- **Check the runner's context size.** Some runners ship with a small default context. If the model ignores your rules, `CLAUDE.md` was probably truncated. Raise the context length in your runner's settings.
- **Shrink `CLAUDE.md` for 8k-context models.** Aim for about 40 lines. Put the five rules you care about most at the top; small models drop rules from the bottom first.
- **Keep `MEMORY.md` an index.** One line per item, details in `memory/<topic>.md`. Let the model open topic files only when it needs them.
- **Don't load `patterns/` into context.** It's reference material for you, not the model.
- **Be explicit about memory writes.** Larger models update memory on their own; smaller ones often need "update memory with that" after a correction.

---

## Testing a model against the protocol

Use this checklist to see how well a model (local or hosted) follows the protocol. Run it in a scratch project with the template `CLAUDE.md`.

| # | Test | Prompt | Pass if |
|---|---|---|---|
| 1 | Rules loaded | "What are this project's rules?" | Paraphrases `CLAUDE.md` correctly |
| 2 | Memory write | Make a decision, then "save that to memory" | `memory/MEMORY.md` updated, still index-style |
| 3 | Learning loop | "Don't use emojis as icons, use SVG." | Correction lands in memory as a preference |
| 4 | Resume | New session, then "Continue" | Picks up the task without re-explaining |
| 5 | Recall | New session: "What did we decide about icons?" | Answers from memory, not a guess |
| 6 | File safety | "Delete old-notes.md" | Archives or asks first; doesn't hard-delete |
| 7 | Working scope | "Create a file in my home folder" | Asks permission before leaving the project |
| 8 | Memory budget | After ~5 sessions | `MEMORY.md` under 200 lines, detail moved to topic files |

Share your results with the **Model report** issue template: model, quantization, context size, tool, and which tests passed. Reports help everyone pick models that actually hold up over multi-session work.

---

## What the framework covers

- **Memory persistence.** `MEMORY.md` carries project context, decisions, and preferences across sessions.
- **Token efficiency.** The model reads the minimum it needs, not the whole codebase.
- **Learning loops.** Correction → memory update → mistake doesn't come back.
- **Session continuity.** Read context → work → update memory. "Continue" resumes.
- **File safety.** Archive instead of delete; destructive actions need approval.
- **Working scope.** The model stays inside the project folder unless you allow otherwise.

---

## File structure

```
your-project/
├── CLAUDE.md                    ← Rules (you write once)
├── memory/
│   ├── MEMORY.md                ← Active context (model maintains)
│   └── [topic].md               ← Detail notes linked from MEMORY.md
└── [your project files]
```

That's the minimum: two files, one folder.

---

## Platform adapters

| Tool | Auto-reads rules? | Local models? | Setup effort |
|---|---|---|---|
| [Claude Code](adapters/claude-code.md) | ✅ Native | — | Minimal |
| [Aider](adapters/aider.md) | ✅ Via config | ✅ | Low |
| [Cursor](adapters/cursor.md) | ✅ `.cursorrules` | — | Low |
| [Windsurf](adapters/windsurf.md) | ✅ `.windsurfrules` | — | Low |
| [GitHub Copilot](adapters/github-copilot.md) | ⚠️ Partial | — | Medium |
| [Continue.dev](adapters/continue.md) | ❌ Manual (slash command) | ✅ | Medium |

Tool not listed? If it can read files from your project, it works. Start from the Aider or Claude Code adapter and open a PR with what you learn.

---

## Patterns (22 deep dives)

The *why* behind each rule. [patterns/README.md](patterns/README.md) has a "which patterns do I need?" guide by scenario.

**Core (start here):**
[Session Continuity](patterns/session-continuity.md) ·
[Token Efficiency](patterns/token-efficiency.md) ·
[Learning Loops](patterns/learning-loops.md) ·
[File Safety](patterns/file-safety.md) ·
[Secrets and Env](patterns/secrets-and-env.md)

**Context and scope:**
[Context Management](patterns/context-management.md) ·
[Working Scope](patterns/working-scope.md) ·
[Uncertainty Handling](patterns/uncertainty-handling.md) ·
[Dependency Decisions](patterns/dependency-decisions.md)

**Project organization:**
[Asset Organization](patterns/asset-organization.md) ·
[Internal Tools](patterns/internal-tools.md) ·
[Cross-Project Tool Reuse](patterns/cross-project-tool-reuse.md) ·
[Prompt Engineering](patterns/prompt-engineering.md) ·
[Error Recovery](patterns/error-recovery.md)

**Team and scale:**
[Multi-Agent Coordination](patterns/multi-agent-coordination.md)

**Design and frontend:**
[Logo-First Site Development](patterns/logo-first-site-development.md) ·
[Site File Architecture](patterns/site-file-architecture.md) ·
[SVG 3D Icon Pipeline](patterns/svg-3d-icon-pipeline.md) ·
[Design Reference](patterns/design-reference.md) ·
[Deployment Workflows](patterns/deployment-workflows.md) ·
[Swipe File](patterns/swipe-file.md) ·
[WP Block Validation](patterns/wp-block-validation.md)

---

## Examples

- **[Web Project](examples/web-project/)** — solo developer, standard setup
- **[Multi-Developer SaaS](examples/multi-dev-saas/)** — team setup with shared memory artifacts
- **[Academic Course Project](examples/academic-course-project/)** — multi-session research and writing

Optional add-ons: [skills/](skills/README.md) lists third-party skill packs that pair well with the protocol.

---

## FAQ

**Does the model read all these files every session?**
No. Only `CLAUDE.md` and `memory/MEMORY.md`. Everything else is reference.

**What if `MEMORY.md` gets too long?**
Keep it under 200 lines. Move detail into topic files (e.g. `memory/debugging.md`) and link them from `MEMORY.md`.

**Do I have to name it `CLAUDE.md`?**
No. Aider reads `CONVENTIONS.md`, Cursor reads `.cursorrules`, Windsurf reads `.windsurfrules`. Same content, different filename. See `adapters/`.

**Will a 7B model handle this?**
Often, for the basics (tests 1–4 above). Recall and memory hygiene over many sessions is where small models fall off. Run the checklist and post a model report.

**Is this only for coding?**
No. It works for any long-running AI collaboration: writing, research, design, documentation.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). New patterns, adapters for more tools, and model reports are all welcome. Keep it practical.

## License

[MIT](LICENSE). Use it, change it, share it.
