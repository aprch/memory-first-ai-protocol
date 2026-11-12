# Changelog

All notable changes to this project are documented here.

Format: `[version] — date — summary`

---

## [1.2.0] — 2026-11-13

### Added
- README section on using the protocol with local models (Ollama, LM Studio, llama.cpp) and small-context tips
- Model conformance checklist in README for testing any model against the protocol
- `.github/ISSUE_TEMPLATE/model_report.md` — template for sharing model test results
- `skills/README.md` — pointers to recommended third-party skill packs (cloned from upstream, not bundled)

### Changed
- `README.md` — restructured for public release; added "Who this is for" and local-model adapter table
- `patterns/design-reference.md` — example made generic
- `.gitignore` — ignores local skill clones, `memory/`, and private project docs

### Removed
- Project-specific skill catalogs from `skills/`

---

## [1.1.0] — 2026-05-09

### Added
- `patterns/secrets-and-env.md` — Handling `.env`, API keys, and secrets safely in memory files
- `patterns/multi-agent-coordination.md` — Running multiple AI sessions simultaneously on the same project
- `patterns/error-recovery.md` — Rollback protocol and post-incident memory updates
- `patterns/dependency-decisions.md` — Tracking package and library choices across sessions
- `patterns/prompt-engineering.md` — Storing reusable prompt templates as project assets
- `adapters/windsurf.md` — Adapter for Windsurf (Codeium) editor
- `adapters/continue.md` — Adapter for Continue.dev (open-source VS Code extension)
- `adapters/aider.md` — Adapter for Aider (CLI-based AI coding assistant)
- `LICENSE` — MIT license
- `CONTRIBUTING.md` — Contribution guidelines
- `.gitignore` — Standard macOS/editor ignores
- `.github/ISSUE_TEMPLATE/` — Bug report and feature request templates
- `.github/PULL_REQUEST_TEMPLATE.md` — PR template
- Use-case navigation section to `patterns/README.md`
- Annotated examples in `templates/CLAUDE.md.template` and `templates/MEMORY.md.template`

### Changed
- `patterns/README.md` — Added "Which patterns do I need?" section organized by scenario
- `README.md` — Updated adapter list, pattern count, and added badges

---

## [1.0.0] — 2026-04-12

### Initial release
- Core two-file system: `CLAUDE.md` + `memory/MEMORY.md`
- Adapters: Claude Code, Cursor, GitHub Copilot
- 17 patterns covering session continuity, token efficiency, file safety, learning loops, context management, working scope, uncertainty handling, asset organization, deployment workflows, design reference, internal tools, cross-project tool reuse, logo-first development, SVG 3D pipeline, swipe file, site file architecture, WordPress block validation
- Examples: Academic course project, Multi-dev SaaS, Web project
- Templates: CLAUDE.md.template, MEMORY.md.template
