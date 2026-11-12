# Skills

Third-party skill packs that pair well with the Memory-First protocol. They are **not bundled** here — clone them from upstream so you get updates and respect each project's license.

```bash
cd your-project
mkdir -p skills && cd skills
git clone https://github.com/blader/humanizer.git
```

`skills/*/` is git-ignored in this repo, so local clones never get committed by accident.

| Skill | What it does | Upstream |
|---|---|---|
| Humanizer | Strips AI-writing tells from prose (filler, em-dash overuse, inflated phrasing) | [blader/humanizer](https://github.com/blader/humanizer) |
| Frontend Slides | Builds single-file, animated HTML decks; converts PPTX | [zarazhangrui/frontend-slides](https://github.com/zarazhangrui/frontend-slides) |
| UI/UX Pro Max | Searchable design-system data: styles, palettes, font pairings, UX rules | [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) |
| Claude Office Skills | Create/edit PPTX, DOCX, XLSX, PDF from the command line | [tfriedel/claude-office-skills](https://github.com/tfriedel/claude-office-skills) |
| Claude Skills | Large multi-tool skill library (Claude Code, Codex, Gemini CLI, Aider, Cursor, …) | [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills) |
| Code Review Graph | Code knowledge graph so reviews read less, spend fewer tokens | [tirth8205/code-review-graph](https://github.com/tirth8205/code-review-graph) |
| Banana Claude | Image generation with Gemini image models, agent as creative director | [AgriciDaniel/banana-claude](https://github.com/AgriciDaniel/banana-claude) |
| Shots for Agents | Share screenshots with agents without pasting base64 into chat (macOS) | [Kalypsokichu-code/shots-for-agents](https://github.com/Kalypsokichu-code/shots-for-agents) |

## Using skills with local models

Most skill packs are just Markdown instructions plus optional scripts, so any model that can read files can follow them. With smaller local models:

- Load **one** skill's `SKILL.md` per task, not the whole folder.
- Reference it from `memory/MEMORY.md` ("For copy polish: read `skills/humanizer/SKILL.md`") instead of pasting it into `CLAUDE.md`.
- Skills that call hosted APIs (e.g. image generation) still need those API keys — see [secrets-and-env](../patterns/secrets-and-env.md).
