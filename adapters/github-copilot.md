# Adapter: GitHub Copilot

GitHub Copilot doesn't read CLAUDE.md or memory files automatically. You need to manually include context or use workarounds.

## Option 1: Copilot Chat with File References

In Copilot Chat, reference your context files:

```
@workspace Read CLAUDE.md and memory/MEMORY.md, then help me with [task]
```

Or include specific files:
```
#file:CLAUDE.md #file:memory/MEMORY.md

[Your question here]
```

## Option 2: Copilot Instructions (Preview Feature)

If you have access to Copilot Instructions:

1. Create `.github/copilot-instructions.md`
2. Copy your CLAUDE.md rules there
3. Copilot will read it for all chat sessions

```markdown
<!-- .github/copilot-instructions.md -->

## Project Rules

[Copy your CLAUDE.md content here]

## Current Context

[Copy relevant MEMORY.md content here]
```

**Limitation:** You'll need to manually update context in this file.

## Option 3: Context in Comments

For inline Copilot (autocomplete), add context in file comments:

```javascript
/**
 * Project Context:
 * - Using TypeScript strict mode
 * - No emojis as icons, use SVG
 * - Error messages should be user-friendly
 */
```

Copilot uses nearby comments for context.

## Option 4: Custom Instructions in Settings

GitHub Copilot settings allow custom instructions:

1. Open VS Code settings
2. Search "copilot instructions"
3. Add your rules as custom instructions

**Limitation:** Same for all projects. Project-specific rules need other methods.

## Adapting the Framework

Since Copilot doesn't auto-read files, adapt the workflow:

### CLAUDE.md → copilot-instructions.md

```bash
cp templates/CLAUDE.md.template .github/copilot-instructions.md
```

### Memory → Manual Updates

Keep MEMORY.md for your reference, but manually include relevant context in chat:

```
Context: We're in Phase 2, working on the dashboard. Last session we finished auth.

[Your question]
```

### Learning Loops → Manual

When Copilot makes a mistake:
1. Correct it in chat
2. Manually add rule to copilot-instructions.md
3. Rule applies to future sessions

## Workflow

```
1. Start chat → Reference #file:CLAUDE.md or include context
2. Work on task
3. End session → Manually update memory/MEMORY.md
4. If corrected → Update .github/copilot-instructions.md
```

More manual than Claude Code, but the patterns still apply.

## Tips

**For Copilot Chat:**
- Always start with context reference
- Use @workspace for codebase-aware responses
- Use #file: for specific file context

**For Inline Copilot:**
- Keep relevant comments near code
- Use JSDoc/docstrings for function context
- Copilot reads open files — keep CLAUDE.md open

**For Better Results:**
- Be explicit about conventions
- Include examples of preferred patterns
- Reference specific files when asking questions

## Limitations

- No automatic memory persistence
- No automatic file reading
- Manual context management required
- Learning loops are manual

Copilot is less suited for this framework than Claude Code, but the patterns still help. Adapt as needed.
