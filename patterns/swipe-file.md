# Pattern: Swipe File

## The Problem

Every project that involves selling something needs copy — listings, headlines, CTAs, pricing descriptions. Without a system:
- You write from scratch every time
- AI-generated copy sounds generic
- You repeat research into "what converts"
- Winning copy from one product never benefits the next

## The Solution

A swipe file: a living collection of proven copy formulas and patterns you update as you learn what works.

### 1. What a Swipe File Is

A swipe file is a folder of copy you've either:
- Written and validated (it worked — keep it)
- Researched from proven sources (structure that's known to convert)
- Adapted from high-performing examples

You don't copy word-for-word. You use the **structure** and swap in your specifics.

### 2. Folder Structure

```
swipe-file/
├── SWIPE-FILE.md           ← Index and update protocol
├── headlines/              ← Hero and section headline formulas
├── listing-copy/           ← Marketplace listing templates
├── cta-copy/               ← Button text and CTA micro-copy
├── pricing-copy/           ← Tier names and pricing descriptions
└── feature-copy/           ← Feature benefit descriptions
```

### 3. The Update Protocol

After every launch or submission:
1. Note what copy you used
2. After 30 days, check what performed (sales, click rate, reviews)
3. Add winning copy to the relevant file with a performance note
4. Remove or flag copy that didn't work

This turns the swipe file into a compounding asset — it gets more valuable over time.

### 4. How AI Uses It

Tell AI to reference the swipe file before writing any marketplace copy:

```markdown
## Copy Rules

Before writing any listing copy, CTA, or headline:
1. Check `swipe-file/` for relevant formulas
2. Adapt the formula to this product
3. Run through humanizer before final use
```

AI reads the swipe file, adapts the proven formula, and produces copy that's structurally sound rather than generically AI-sounding.

### 5. Seeding the Swipe File

If you're starting from scratch, seed it with:
- Formulas from established copywriting frameworks (AIDA, PAS, FAB)
- Structures from the highest-converting listings in your marketplace (analyse, don't copy)
- Your own copy that's worked in other contexts

## How to Implement

Create `swipe-file/` in your project:

```bash
mkdir -p swipe-file/{headlines,listing-copy,cta-copy,pricing-copy,feature-copy}
```

Add to `CLAUDE.md`:
```markdown
## Copy Protocol
Before writing any marketplace or sales copy:
1. Check swipe-file/ for relevant formulas
2. Adapt formula to this product's specifics
3. Run final copy through humanizer
```

Add to `MEMORY.md`:
```markdown
## Swipe File
Location: `swipe-file/`
Update after every submission — add what worked, flag what didn't.
```

## Measuring Success

You're doing it right if:
- You never write copy structure from scratch
- Each product launch produces better copy than the last
- AI references actual proven formulas instead of generating generically
- The swipe file grows after every submission
