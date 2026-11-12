# Pattern: Internal Tools

## The Problem

During a project, you create scripts, utilities, automation. Without structure:
- Tools get lost in random folders
- No one remembers how to use them
- Future sessions recreate existing tools
- Onboarding new collaborators requires verbal explanation

## The Solution

A dedicated tools folder with an index file.

### 1. Tools Folder Structure

```
your-project/
├── tools/
│   ├── TOOLS-INDEX.md          ← Quick reference for all tools
│   ├── carousel-export/        ← Tool 1
│   │   ├── README.md
│   │   ├── package.json
│   │   └── export-slides.js
│   ├── pdf-generator/          ← Tool 2
│   │   └── ...
│   └── data-migration/         ← Tool 3
│       └── ...
```

### 2. TOOLS-INDEX.md Format

Quick reference — scannable in seconds:

```markdown
# Project Tools Index

## Carousel Export (`tools/carousel-export/`)
**What:** Exports HTML carousel slides to PNG images
**Setup:** `npm install`
**Usage:** `node export-slides.js input.html ./output`

## PDF Generator (`tools/pdf-generator/`)
**What:** Converts markdown docs to branded PDFs
**Setup:** `pip install -r requirements.txt`
**Usage:** `python generate.py docs/`

## Quick Commands
\`\`\`bash
# Export carousel
cd tools/carousel-export && node export-slides.js ../../src/carousel.html ./out

# Generate PDF
cd tools/pdf-generator && python generate.py ../../docs/
\`\`\`
```

### 3. Individual Tool READMEs

Each tool gets its own README with:
- What it does
- Setup instructions
- Usage examples
- Notes/caveats

```markdown
# Carousel Export Tool

Exports HTML carousel slides to individual PNG images.

## Setup
\`\`\`bash
cd tools/carousel-export
npm install
\`\`\`

## Usage
\`\`\`bash
node export-slides.js <path-to-html> [output-dir]
\`\`\`

## Example
\`\`\`bash
node export-slides.js ../../marketing/carousel-03.html ./carousel3
\`\`\`

## Output
Creates numbered PNG files (1.png, 2.png...) at 1080x1080px.
```

### 4. Tool Output Organization

Tool outputs follow the Asset Organization pattern:

```
tools/carousel-export/
├── README.md
├── export-slides.js
├── carousel1/          ← Output folder
│   ├── 1.png
│   └── captions.md
├── carousel2/
└── carousel3/
    ├── 1.png
    ├── 2.png
    └── captions.md
```

## How to Implement

When creating a tool:

1. **Create folder** in `tools/`
2. **Add README.md** with setup + usage
3. **Update TOOLS-INDEX.md** with quick reference
4. **Organize outputs** in subfolders with context files

Add to CLAUDE.md:
```markdown
## Internal Tools
- Tools live in `tools/` folder
- Each tool has its own README
- TOOLS-INDEX.md has quick reference for all
- Tool outputs organized in subfolders with context
```

## When to Create a Tool

Create a documented tool when:
- You'll use the script more than once
- Someone else might need it
- It took more than 5 minutes to write
- It has dependencies or setup steps

Don't bother for:
- One-off commands
- Simple shell aliases
- Things built into your framework

## Why This Matters

1. **Discoverability** — TOOLS-INDEX.md shows what exists
2. **Reusability** — Future sessions find and use existing tools
3. **Onboarding** — New collaborators get up to speed fast
4. **AI Context** — AI can reference and run tools appropriately

## Measuring Success

You're doing it right if:
- You never recreate a tool that already exists
- New team members can use tools without asking
- TOOLS-INDEX.md is current and accurate
- AI knows about and uses your tools
