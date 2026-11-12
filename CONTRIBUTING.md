# Contributing to Memory-First AI Protocol

Thanks for taking the time to contribute. This project is a living collection of patterns, adapters, and templates — it improves when people share what actually works in practice.

## What We're Looking For

### New Patterns
A pattern belongs here if it solves a recurring problem in AI-assisted development and can be stated as:
- **Problem** — what goes wrong without the pattern
- **Solution** — the specific practice that fixes it
- **Success metric** — how you know it's working

Patterns should be tool-agnostic where possible. If it only applies to one AI tool, it belongs in `adapters/` instead.

### New Adapters
An adapter documents how to apply this framework with a specific AI tool. Include:
- What the tool reads automatically vs. manually
- Setup steps (copy X to Y, reference Z in chat)
- Known limitations
- Tool-specific tips

### Improved Examples
Real-world examples showing the framework in action on a complete project. Not hypothetical — show actual CLAUDE.md, MEMORY.md content, and what happened.

### Template Improvements
Better starter templates, annotated with what goes where and why.

### Model Reports
Ran the checklist in the README against a local or hosted model? Open an issue with the **Model report** template. Include model, quantization, context length, runner, and tool so others can reproduce it.

---

## How to Contribute

1. **Fork** the repo
2. **Create a branch** — `pattern/secrets-handling`, `adapter/windsurf`, `example/mobile-app`, etc.
3. **Add your file** — follow the existing structure and tone (direct, concise, practical)
4. **Open a PR** — describe what problem your contribution solves

---

## File Naming Conventions

| Type | Location | Naming |
|---|---|---|
| New pattern | `patterns/` | `kebab-case.md` |
| New adapter | `adapters/` | `tool-name.md` |
| New example | `examples/project-type/` | `README.md` |

---

## Writing Style

- **Direct, not academic.** Say "Archive don't delete" not "It is recommended that deletion be avoided in favor of archiving."
- **Rules, not explanations.** State what to do. Don't explain the psychology behind it.
- **Real examples.** Show actual file content, not pseudocode.
- **Short.** If a pattern can be stated in 50 lines, don't write 200.

---

## What Doesn't Belong

- Tool configuration unrelated to AI context management
- Patterns that only work in very narrow, project-specific scenarios
- Promotional content for specific AI tools or services

---

## Questions?

Open an issue describing what you're trying to add and we can discuss before you write it.
