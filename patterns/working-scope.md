# Pattern: Working Scope

## The Problem

AI has access to your filesystem. Without boundaries, AI might:
- Create files outside your project
- Read sensitive files from other directories
- Make changes in unexpected locations
- Lose track of where it's working

Even with good intentions (like creating a separate repo), stepping outside the project folder without asking can cause confusion and trust issues.

## The Solution

Define a working scope. Stay in it. Ask before leaving.

### 1. Default to Project Root

AI should work within the project folder it was started in.

```
✅ /my-project/src/...           (inside project)
✅ /my-project/memory/...        (inside project)
❌ /other-folder/...             (outside — ask first)
❌ ~/Desktop/...                  (outside — ask first)
```

### 2. Ask Before Leaving Scope

If a task requires accessing files outside the project:

```
❌ Bad: "I'll create that at /Users/name/other-folder/"
        *Creates without asking*

✅ Good: "This should be a separate repo. I'd create it at:
         /Users/name/other-folder/new-project/

         Is that okay, or do you want it somewhere else?"
        *Waits for confirmation*
```

### 3. Confirm Every Time

Even if user approved leaving scope once, ask again next time.

```
Session 1: "Can I create /other-folder/file.md?" → User: "Yes"
Session 2: "Can I access /other-folder/ again?" → Ask, don't assume

Previous approval ≠ permanent access
```

### 4. State Where You're Working

When starting a session or switching contexts, be clear:

```
"Working in: /Users/name/my-project/
 I'll stay within this folder unless you ask otherwise."
```

### 5. Relative Paths Within Project

Use relative paths when possible to stay scope-aware:

```
✅ Good: ./src/components/Button.tsx
✅ Good: ./memory/MEMORY.md
❌ Risky: /full/absolute/path/to/file (harder to notice scope violation)
```

## How to Implement

Add to your CLAUDE.md:

```markdown
## Working Scope

- Stay within project folder: [PROJECT ROOT PATH]
- Access outside ONLY if explicitly requested
- Ask permission before creating files outside project
- Previous approval doesn't carry over to new sessions
```

## Common Scenarios

**Creating a new repo:**
```
❌ Bad: Create it outside project without asking
✅ Good: "This should be separate. Where should I create it?"
```

**Reading reference files:**
```
❌ Bad: Read ~/Documents/notes.md without asking
✅ Good: "I need to check a file outside this project. Can I read ~/Documents/notes.md?"
```

**Installing dependencies:**
```
✅ Usually fine: npm install (stays in project via node_modules)
⚠️ Ask first: Global installs (npm install -g) affect system
```

**Git operations:**
```
✅ Within project: git status, git commit
⚠️ Ask first: Cloning other repos, pushing to different remotes
```

## Why This Matters

1. **Trust** — User knows exactly where AI is working
2. **Safety** — Prevents accidental changes to other projects
3. **Clarity** — No confusion about file locations
4. **Reversibility** — Easier to undo changes when they're contained

## Measuring Success

You're doing it right if:
- All new files created within project folder
- User is never surprised by file locations
- Scope violations are rare and always approved
- AI explicitly states working directory when relevant
