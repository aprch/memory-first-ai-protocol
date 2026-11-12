# Example: Academic Course Project (Multi-Session Assignment)

This shows how the Memory-First AI Protocol works for a university assignment that spans multiple sessions — with tool setup, content drafting, design, and output generation across separate conversations.

## The Context

A student working on a major oral presentation for a graduate professional course. The assignment required:
- An 11-slide PowerPoint with speaker notes
- A specific citation style + a reference verification checklist
- Application of a course-specified decision-making framework
- Professional register (executive audience, not academic)

The work took multiple sessions. Without persistent memory, every session would have required re-explaining the scenario, the framework, the tools, the referencing requirements, and the preferred working style.

---

## What Made This Work

### 1. CLAUDE.md as the full project spec

The CLAUDE.md file contained everything the AI needed to resume instantly:
- The assessment brief (scenario, slide structure, marking criteria)
- All approved references with full citations
- Tool paths and workflow steps
- Jurisdiction-specific legal context (not generic national-level)
- Style preferences ("discuss before building", "humanize all body text")

**Key insight:** A detailed CLAUDE.md eliminates the 10-minute re-briefing at the start of every session.

### 2. "Discuss before building" as a locked preference

The student explicitly set this rule early. The AI never wrote a single slide until all content was approved. This prevented wasted rebuilds and kept the student in control.

Memory entry that enforced this:
> "Always align on content and structure before writing slides. Humanizer pass mandatory — all slide body text must pass through the humanizer before final output."

### 3. Reference tracking in CLAUDE.md (not memory)

All references — with full citations, DOIs, source files, and relevance notes — were kept in CLAUDE.md rather than memory. This meant the AI always had exact citations available without needing to search.

**Lesson learned:** Even with references tracked, always verify author names against the actual source file (library database page, journal page). In one case, a paper attributed to "[Author A et al.]" was actually authored by "[Author B et al.]" — caught only when the original database record was read directly.

### 4. Session continuity via summary + CLAUDE.md

When a session hit the context limit, the next session resumed by:
1. Reading the context summary (auto-generated)
2. Reading CLAUDE.md
3. Checking the output directory for in-progress files

No manual re-briefing needed. The AI picked up mid-task (an npm install that had failed due to PATH issues) and completed it.

### 5. Tool environment documented in CLAUDE.md

The CLAUDE.md included the npm setup command, output directory path, and file naming convention. When the tool environment needed debugging (npm not in PATH on macOS), the AI found the correct path without asking the user.

---

## File Structure

```
course-project/
├── CLAUDE.md                    ← Full project spec (brief, references, tools, workflow)
├── memory/
│   ├── MEMORY.md                ← Index (2 entries)
│   └── project_notes.md         ← Status, tools, framework, due date
├── assessment-1-output/         ← Final output destination
│   └── Presentation.pptx
├── references/                  ← Source files from library databases
├── claude-office-skills/        ← HTML→PPTX tool (cloned repo)
│   └── outputs/assessment-presentation/
│       ├── slide01.html … slide11.html
│       ├── generate.cjs
│       └── presentation.pptx
├── humanizer/                   ← AI-writing pattern removal tool
└── frontend-slides/             ← Design presets and templates
```

---

## CLAUDE.md Structure (condensed)

```markdown
# CLAUDE.md — [Course Name] Workspace

## Project Overview
Course: [Course Name]
Working Directory: [path]

## Assessment Details
- Due: [submission deadline]
- Format: PDF of slides + speaker notes
- Marks: 100 = 50% of grade
- Referencing: [citation style] + reference verification checklist

## Slide Structure (11 slides)
1. Title … 11. References

## Tools
- claude-office-skills/ — HTML→PPTX workflow
- humanizer/ — remove AI writing patterns
- frontend-slides/ — design templates

## Workflow
1. Content first — draft, discuss, approve
2. Humanize — all body text
3. Design — pick palette
4. Build — write HTML slide files
5. Generate — run html2pptx
6. Validate — thumbnail check
7. Deliver — copy to submission folder

## Approach
- Discuss before building
- Humanizer pass mandatory
- No AI tells in final text
- Professional register (executive audience)

## Legal/Regulatory Context
- [National privacy legislation]
- [State/territory-level regulatory Act] — mandatory reporting
- [Professional regulatory body] Code of Conduct
- NOTE: [state/territory] complaints go to [state body], not [national body] directly

## References
[Full citations for every source with DOIs and source file paths]

## Resume Instructions
1. Read this file
2. Check assessment-1-output/ for existing files
3. Check claude-office-skills/outputs/assessment-presentation/ for in-progress work
4. Ask where to pick up
```

---

## Memory Files (minimal — CLAUDE.md did the heavy lifting)

**MEMORY.md** (index only):
```markdown
# Memory Index
- [Course Assessment 1](project_notes.md) — status and tool notes
```

**project_notes.md**:
```markdown
Status: Presentation complete. PPTX in submission folder.
Due: [submission deadline].
Framework: [course-specified framework].
Pending: Fill in name/student number. Export PDF with notes. Complete reference checklist.
```

---

## Patterns Demonstrated

| Pattern | How it showed up |
|---|---|
| Session continuity | Resumed mid-task after context limit with zero re-briefing |
| Learning loops | Jurisdiction error ([state body] vs [national body]) caught once, locked in CLAUDE.md |
| Token efficiency | CLAUDE.md loaded once; memory files stayed under 200 lines |
| File safety | Output goes to separate `outputs/` dir; final copy to submission folder only when ready |
| Working scope | All work in project folder; tool repos cloned inside the project |

---

## Lessons for Other Academic Projects

1. **Put the full brief in CLAUDE.md.** Paste the assessment description, marking criteria, referencing requirements, and due date. You will not remember to mention all of this each session — but the AI will read it.

2. **Track references in CLAUDE.md, not memory.** References are stable. Keep exact citations there. Verify author names against the actual source — never trust a summary or a cached version.

3. **Set working style rules early and lock them.** "Discuss before building", "humanize all text", "professional not academic tone" — set these in session 1, put them in CLAUDE.md, and they hold for every session.

4. **Use jurisdiction-specific legal context.** Generic national-level law is often wrong for your situation. Specify the state/territory and the correct regulatory body by name.

5. **Reference verification checklists are yours to complete.** The AI can pre-populate full citations and in-text references. But the verification steps (does the DOI work, does the source actually say what you claimed) are yours to personally check. That is the point of the checklist.
