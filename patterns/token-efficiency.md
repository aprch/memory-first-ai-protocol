# Pattern: Token Efficiency

## The Problem

AI context windows are limited. Every file you read, every explanation you type, every retry burns tokens. Most people waste 50%+ of their context on:

- Re-reading files they already know
- Explaining things they've explained before
- AI reading irrelevant documentation
- Verbose back-and-forth that could be one message
- **Iterating on the same change across multiple files**

## The Solution

Minimize what AI reads. Maximize what AI remembers.

### 1. Lean Memory Files

Keep `MEMORY.md` under 200 lines. It's an index, not a journal.

```
❌ Bad: 500-line MEMORY.md with full conversation history
✅ Good: 100-line index linking to topic files when needed
```

### 2. Rules, Not Explanations

CLAUDE.md should have rules, not explanations of rules.

```
❌ Bad: "Because accessibility is important and users need to navigate
        with keyboards, we should ensure all interactive elements have
        visible focus states..."

✅ Good: "Focus states visible for keyboard nav"
```

### 3. Bash for Bulk Operations

Reading 10 files individually = 10 tool calls = overhead.
One bash command = 1 tool call.

```
❌ Bad: Read file1, Read file2, Read file3...
✅ Good: cat file1 file2 file3 (when you need raw content)
✅ Better: grep -r "pattern" ./src (when you need to find something)
```

### 4. Don't Re-Read Known Files

If AI read a file earlier in the session, it knows what's there. Don't read it again unless it changed.

### 5. Edit Existing, Don't Create New

Every new file = more to track = more to read later.

```
❌ Bad: Create notes-v2.md, notes-v3.md, notes-final.md
✅ Good: Edit notes.md in place
```

### 6. Batch Similar Changes (Added 2026-04-07)

When making the same type of change across multiple files, plan first then execute in batch.

```
❌ Bad: Edit file1 → Test → Edit file2 → Test → Edit file3 → Test
        (Multiple prompts, repeated explanations)

✅ Good:
   1. List all files that need the change
   2. Read all files in parallel
   3. Apply changes with replace_all where possible
   4. Verify once at the end
```

**Real Example — Clean URL Updates:**
```bash
# First: Find all files with .html links
grep -rn '\.html' site/*.html | grep 'href='

# Then: Batch replace in each file
Edit with replace_all=true for each pattern
```

### 7. Check Scope Before Starting

Before any multi-file task, check the full scope:

```
❌ Bad: Start editing index.html → Realize services.html needs same change
        → Edit services.html → Realize privacy.html too → ...

✅ Good:
   1. "How many files have this pattern?"
   2. List them all upfront
   3. Plan the change
   4. Execute in one pass
```

### 8. Document Hidden Files & Edge Cases

Common causes of multiple prompts:
- Hidden files (`.htaccess`) not visible in Finder
- Different file locations than expected
- URL format inconsistencies

Add to MEMORY.md:
```markdown
## Hidden Files
- `.htaccess` — Cmd+Shift+. to see in Finder

## URL Format
- Clean URLs: `services` not `services.html`
- Homepage: `/`
```

## How to Implement

Add to your CLAUDE.md:

```markdown
## Token Efficiency

- Use bash for bulk operations
- Don't create unnecessary .md files — edit existing
- Read existing docs before writing new ones
- MEMORY.md stays under 200 lines (index only)
- Batch similar changes across files
- Check full scope before starting multi-file tasks
```

## Measuring Success

You're doing it right if:
- Sessions feel faster
- AI doesn't ask for context you've given before
- You're not hitting context limits mid-task
- MEMORY.md is lean but useful
- Multi-file changes happen in one pass, not multiple prompts
