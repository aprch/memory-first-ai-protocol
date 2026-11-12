# Pattern: Learning Loops

## The Problem

AI makes a mistake. You correct it. Next session? Same mistake. AI doesn't learn from corrections — unless you make it.

Common repeat offenders:
- Using wrong formatting/style
- Suggesting approaches you've rejected
- Missing project-specific conventions
- Forgetting your preferences

## The Solution

Corrections become rules. Rules persist in memory.

### 1. The Correction → Memory Loop

When you correct AI:

```
You: "No, don't use emojis as icons. Use SVG."
AI: *Updates MEMORY.md with this preference*
AI: *Never uses emojis as icons again*
```

This has to be explicit. Tell AI to update memory when corrected.

### 2. Locked Preferences Section

In MEMORY.md, keep a section for learned rules:

```markdown
## Locked Preferences

[Rules learned from corrections — DO NOT violate]

- Never use emojis as structural icons (2024-01-10)
- Always ask before deleting files (2024-01-08)
- Use TypeScript strict mode (2024-01-05)
```

AI reads this every session. Rules accumulate over time.

### 3. Immediate Updates

Don't wait until end of session to update memory. Update immediately after correction.

```
❌ Bad: "I'll remember that for next time"
✅ Good: *Updates MEMORY.md right now*
```

### 4. The Memory Update Instruction

Make it explicit in CLAUDE.md:

```markdown
## Learning Protocol

When user corrects a mistake:
1. Acknowledge the correction
2. Update memory/MEMORY.md immediately
3. Add to "Locked Preferences" with date
4. Never repeat the mistake
```

### 5. Remove Wrong Memories

If you stored something incorrect, remove it:

```
You: "Actually, we ARE using emojis now. Remove that rule."
AI: *Removes from Locked Preferences*
```

Memory should be accurate, not just accumulated.

## How to Implement

Add to your CLAUDE.md:

```markdown
## Learning Protocol

When corrected:
1. Update memory/MEMORY.md immediately
2. Add correction to "Locked Preferences"
3. Never repeat the corrected behavior

When told to forget:
1. Find and remove the relevant memory
2. Confirm removal
```

Add to your MEMORY.md template:

```markdown
## Locked Preferences

[Rules learned from corrections — DO NOT violate]

- [Date]: [Rule learned]
```

## Measuring Success

You're doing it right if:
- AI doesn't repeat corrected mistakes
- Locked Preferences grows over time
- You correct less as sessions progress
- AI anticipates your preferences
