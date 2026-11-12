# Pattern: File Safety

## The Problem

AI can delete files. AI can overwrite work. AI can make destructive changes that are hard to undo.

Horror stories:
- "AI deleted my entire src folder"
- "AI overwrote my config with a template"
- "AI removed code I spent hours on"

## The Solution

Conservative defaults. Archive over delete. User approves destructive actions.

### 1. Archive, Don't Delete

When removing something, move it instead of deleting:

```
❌ Bad: rm old-file.js
✅ Good: mv old-file.js archive/old-file.js
```

If uncertain, use a suffix:

```
❌ Bad: rm maybe-unused.js
✅ Good: mv maybe-unused.js maybe-unused-..delete.js
```

The `-..delete` suffix signals "probably safe to delete, but review first."

### 2. User Approves Destructive Actions

Never auto-execute:
- File deletions
- Force pushes
- Database drops
- Config overwrites

Ask first. Wait for approval.

### 3. Read Before Write

Never overwrite a file without reading it first:

```
❌ Bad: Write new content to existing file (blind overwrite)
✅ Good: Read file → Understand what's there → Edit specific parts
```

### 4. Direct Approval Pattern

Instead of asking "should I delete this?", show the action:

```
❌ Bad: "Should I remove these 5 files?"
✅ Good: "Renaming these for deletion review:
         - old-config.js → old-config-..delete.js
         - unused-util.js → unused-util-..delete.js
         You can delete them after review or restore by removing suffix."
```

User can approve by deleting, or restore by renaming back.

### 5. Git as Safety Net

Before major changes:
- Commit current state (or ensure clean working tree)
- Make changes
- User can git checkout/reset if wrong

Don't rely on this — but it's there.

## How to Implement

Add to your CLAUDE.md:

```markdown
## File Safety

- Archive old files (don't delete unless certain)
- Use `-..delete` suffix for uncertain deletions
- Read files before overwriting
- Ask approval before destructive actions
- User approves/reverts changes directly
```

## Common Scenarios

**Cleaning up old files:**
```
Don't: rm -rf old/
Do: mv old/ archive/old-YYYYMMDD/
```

**Uncertain if file is used:**
```
Don't: Delete it and see if anything breaks
Do: Rename to file-..delete.js, run tests, delete if pass
```

**Refactoring large sections:**
```
Don't: Rewrite entire file
Do: Make incremental edits, commit between changes
```

## Measuring Success

You're doing it right if:
- You've never lost work to AI
- archive/ folder exists and has old versions
- You feel safe letting AI edit files
- Recovery is always possible
