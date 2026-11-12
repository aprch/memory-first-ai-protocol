# Pattern: Context Management

## The Problem

AI context windows are finite. Fill them with irrelevant information and AI:
- Loses focus on the actual task
- Forgets important details
- Makes connections to wrong context
- Runs out of space for actual work

## The Solution

Curate what AI knows. Index over dump. Relevant over complete.

### 1. Index, Don't Dump

MEMORY.md is an index, not a complete record.

```
❌ Bad: Entire conversation history in MEMORY.md
✅ Good: Summary with links to details

## Project Map
- `src/auth/` — Authentication system
- `src/api/` — API endpoints
- See `memory/api-notes.md` for endpoint details
```

AI reads the index. Reads details only when needed.

### 2. Topic Files for Depth

When something needs detail, create a topic file:

```
memory/
├── MEMORY.md          ← Index (always read)
├── debugging.md       ← Debugging notes (read when debugging)
├── api-notes.md       ← API details (read when doing API work)
└── decisions.md       ← Architecture decisions (read when relevant)
```

Reference from MEMORY.md:

```markdown
## Topic Files
- `debugging.md` — Known issues and solutions
- `api-notes.md` — Endpoint documentation
```

AI reads topic files only when the task requires them.

### 3. Relevance Over Completeness

Not everything needs to be in memory.

```
✅ Keep: Project structure, active work, key decisions, locked preferences
❌ Skip: Completed tasks older than 2 weeks, experimental branches abandoned,
        detailed notes from resolved issues
```

Prune regularly. Memory should be current.

### 4. The 200-Line Rule

MEMORY.md over 200 lines = too long. When approaching limit:
1. Move details to topic files
2. Archive old session notes
3. Summarize verbose sections
4. Delete obsolete information

### 5. Project Mapping

Keep a mental model of the project in MEMORY.md:

```markdown
## Project Map

project/
├── src/
│   ├── components/    ← React components
│   ├── api/           ← Backend routes
│   └── utils/         ← Shared utilities
├── tests/             ← Test files mirror src/
└── docs/              ← Documentation
```

AI can navigate without reading everything.

## How to Implement

Add to your CLAUDE.md:

```markdown
## Context Management

- MEMORY.md stays under 200 lines (index only)
- Create topic files for detailed notes
- Prune memory regularly — keep it current
- Project map in memory for navigation
```

Add structure to MEMORY.md:

```markdown
## Project Map
[Directory structure with brief descriptions]

## Topic Files
[Links to detailed memory files]

## Session Notes
[Last 3-5 sessions only, oldest deleted]
```

## Measuring Success

You're doing it right if:
- MEMORY.md is under 200 lines
- AI can find things without reading entire codebase
- Irrelevant context doesn't pollute responses
- You don't explain project structure repeatedly
