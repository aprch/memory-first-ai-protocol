# Pattern: Uncertainty Handling

## The Problem

AI hallucinates. It states things confidently that aren't true. It guesses when it should ask. It assumes when it should verify.

Common hallucinations:
- "This file exists at..." (it doesn't)
- "The function does X" (it does Y)
- "Based on the code..." (didn't read the code)
- Making up APIs, syntax, or features

## The Solution

Be explicit about uncertainty. Verify before stating. Ask instead of guess.

### 1. Confidence Signals

Use language that matches your certainty level:

```
✅ High confidence (verified): "The file is at src/utils/auth.ts"
✅ Medium confidence (likely): "I believe this is handled in the auth module"
✅ Low confidence (uncertain): "I'm not sure, but it might be in utils/"
❌ False confidence: "The API returns X" (without checking)
```

### 2. Verify Before Stating

Don't claim something exists without checking:

```
❌ Bad: "The config file has a debug flag you can enable"
        (Didn't read the file, might not exist)

✅ Good: *Reads config file first*
        "The config file has debug: false on line 23. Want me to enable it?"
```

### 3. Ask Instead of Guess

When multiple paths are valid, ask:

```
❌ Bad: *Picks an approach and implements it*
        (Might not be what user wanted)

✅ Good: "I can do this two ways:
         A) Add to existing file
         B) Create new module
         Which do you prefer?"
```

### 4. Flag Assumptions

When you must assume, say so:

```
✅ "I'm assuming you want this in TypeScript. Let me know if not."
✅ "I don't see a test file, so I'm assuming tests aren't required here."
```

### 5. Admit When You Don't Know

```
❌ Bad: Make up an answer that sounds plausible
✅ Good: "I don't know. Want me to search for it?"
```

## How to Implement

Add to your CLAUDE.md:

```markdown
## Accuracy Protocol

- State uncertainty explicitly ("I believe...", "I'm not certain...")
- Verify before stating — read files, don't assume
- Ask when multiple valid approaches exist
- Flag assumptions explicitly
- Admit "I don't know" rather than guessing
```

Add to MEMORY.md Locked Preferences:

```markdown
- Don't state things confidently without verification
- Ask rather than guess when uncertain
- Flag assumptions explicitly
```

## The Tradeoff

Being explicit about uncertainty:
- **Pro:** Fewer wrong answers, more trust, easier to correct
- **Con:** Slightly more back-and-forth, feels less "smart"

The tradeoff is worth it. A confident wrong answer wastes more time than asking a clarifying question.

## Measuring Success

You're doing it right if:
- User rarely says "that's not right"
- Assumptions are flagged before acting
- Uncertain statements use hedging language
- AI asks clarifying questions instead of guessing
