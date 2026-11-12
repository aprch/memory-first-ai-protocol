# Error Recovery

**Solves:** When AI breaks something significantly — wrong file deleted, bad refactor, broken build, corrupted state — how to recover and make sure it never happens the same way again.

---

## The Problem

AI makes mistakes. Sometimes small (wrong variable name), sometimes large (deleted the wrong files, broke the database schema, rewrote a component that worked fine). Without a protocol:

- You spend time figuring out what changed
- You don't know which files to restore
- The same mistake happens in a future session because nothing was learned
- Panic leads to more mistakes (applying fixes without understanding what broke)

---

## The Recovery Protocol

### Step 1: Stop immediately

When something breaks, don't ask AI to "fix it" right away. First understand what happened.

Tell the AI: **"Stop. Something broke. Don't touch any files until we figure out what happened."**

### Step 2: Assess what changed

```bash
# See what files changed in this session
git diff --name-only

# See the full diff
git diff

# See what was committed recently
git log --oneline -10
```

If you weren't using git, list recently modified files:
```bash
find . -newer CLAUDE.md -not -path "./.git/*" -not -path "./node_modules/*"
```

### Step 3: Restore from git

If you committed before the session started (best case):
```bash
# Restore a specific file
git checkout HEAD -- src/components/Button.tsx

# Restore everything to last commit (nuclear)
git reset --hard HEAD
```

If the AI made commits during the session:
```bash
# See what commits were made
git log --oneline

# Undo the last N commits, keep changes unstaged
git reset HEAD~N

# Review, then selectively restore
```

### Step 4: Document what happened

Before moving on, add to `memory/MEMORY.md` under a new section:

```markdown
## Incidents

### [DATE] — [Brief title, e.g., "Auth middleware deleted"]
**What happened:** AI deleted `src/middleware/auth.ts` while cleaning up unused files.
**Root cause:** Instruction was "remove unused files" — AI misidentified auth.ts as unused.
**Resolution:** Restored from git (`git checkout HEAD -- src/middleware/auth.ts`).
**Prevention:** Never run "remove unused files" as an open-ended instruction. Always list specific files first.
```

### Step 5: Add a Locked Preference immediately

The specific mistake becomes a rule. Add to `memory/MEMORY.md` Locked Preferences section:

```markdown
## Locked Preferences

- 2026-05-01 — Never delete or rename files without listing them first for review. "Clean up" requests require explicit file list approval.
```

And if relevant, update `CLAUDE.md`:

```markdown
## File Safety

- Before any "cleanup" or "remove unused" operation: list all files that would be affected and wait for approval
```

---

## Prevention: Commit Before Big Changes

The most effective recovery tool is a clean git state before any risky operation.

Add this to your CLAUDE.md:
```markdown
## Pre-Delivery Checklist
- [ ] Git is clean (or changes are committed) before starting large refactors
```

Tell AI explicitly: **"Before we start this refactor, commit the current state."**

```bash
git add -A && git commit -m "snapshot before refactor: [description]"
```

This costs 5 seconds and makes any recovery a one-liner.

---

## If You Don't Have Git

Create manual snapshots before risky work:
```bash
# Copy the relevant folder with a timestamp
cp -r src/ src-backup-$(date +%Y%m%d-%H%M)/
```

Add `*-backup-*/` to `.gitignore` so backups don't commit.

---

## Classifying Incidents

Not all errors are equal. Calibrate your response:

| Severity | Example | Response |
|---|---|---|
| Minor | Wrong variable name, typo | Fix in place, no incident log needed |
| Moderate | Broke a component, wrong logic | Restore from git, note in session log |
| Major | Deleted files, broke build/DB | Full protocol above, Locked Preference added |
| Critical | Production data affected | Stop all AI work, fix manually, post-mortem |

For Critical: AI should not be involved in the recovery. Fix it yourself or with a human colleague, then document what happened and update rules.

---

## Success Metric

- Every major incident gets documented in MEMORY.md
- Locked Preferences grows after incidents — mistakes don't repeat
- You always have a clean git commit before risky operations
- Recovery time decreases as the prevention rules improve

---

## See Also

- [File Safety](file-safety.md) — archive over delete, read before write
- [Learning Loops](learning-loops.md) — corrections become rules immediately
- [Uncertainty Handling](uncertainty-handling.md) — AI flags risky operations before doing them
