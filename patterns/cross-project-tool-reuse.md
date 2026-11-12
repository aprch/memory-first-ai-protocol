# Pattern: Cross-Project Tool Reuse

## The Problem

When starting a new project, you build tools from scratch — even when equivalent tools already exist in other projects on the same machine. This wastes time and fragments your tool ecosystem.

Common missed opportunities:
- Design system generators used in Project A, rebuilt from scratch in Project B
- Copy polish tools already installed elsewhere — not carried over
- Security review pipelines duplicated across projects
- Utilities written twice because AI doesn't know they exist

## The Solution

Audit your existing tool ecosystem before starting a new project. Identify which tools transfer. Copy or symlink them in.

### 1. The Audit Question

Before building any tooling for a new project, ask:

> "Do I already have a tool that does this in another project?"

Check your other projects' `tools/` folders. Most utility tools (design systems, review pipelines, copy polish, file processors) are project-agnostic and reuse cleanly.

### 2. Which Tools Transfer

**High reuse potential (copy these):**
- Design system generators (colors, typography, spacing)
- Code quality / review pipelines
- Copy and documentation polish tools
- File format converters (PDF, PPTX, DOCX)
- Security audit scripts

**Low reuse potential (rebuild per project):**
- Project-specific data pipelines
- Framework-specific build tools
- Tools with hardcoded project paths or credentials

### 3. How to Copy

```bash
# Copy a tool from another project into the current project
cp -r /path/to/other-project/tools/tool-name ./tools/tool-name
```

Or symlink if you want updates to propagate:
```bash
ln -s /path/to/other-project/tools/tool-name ./tools/tool-name
```

### 4. Document It

When you copy a tool, note the source in `TOOLS-INDEX.md`:

```markdown
## Design System Tool (`tools/ui-ux-pro-max/`)
**Source:** Copied from [other-project] — reused as-is
**What:** Generates design tokens (colors, fonts, spacing)
**Usage:** `python3 tools/ui-ux-pro-max/scripts/search.py "<query>" --design-system`
```

This prevents future sessions from thinking the tool is project-specific or wondering where it came from.

### 5. Add to CLAUDE.md

Tell AI what tools exist so it uses them instead of recreating them:

```markdown
## Available Tools

- `tools/design-system/` — generates color palettes and font pairings
- `tools/copy-humanizer/` — removes AI writing patterns from docs
- `tools/review-pipeline/` — security + standards audit before submission
```

## How to Implement

At the start of a new project:

1. List tools in your other projects: `ls ~/projects/*/tools/`
2. Identify which are reusable (see criteria above)
3. Copy them into `tools/`
4. Document in `TOOLS-INDEX.md` with source noted
5. Add to `CLAUDE.md` so AI knows they exist

## Why This Matters

1. **Speed** — Don't rebuild what already works
2. **Consistency** — Same tools = same output quality across projects
3. **Reduced hallucination** — AI uses known tools instead of inventing approaches
4. **Compounding value** — Your tool ecosystem grows once, benefits all projects

## Measuring Success

You're doing it right if:
- New projects start with a working tool ecosystem, not an empty `tools/` folder
- AI references existing tools instead of suggesting you build new ones
- Time to first working output decreases per new project
- Your tools folder across projects converges toward a personal toolkit
