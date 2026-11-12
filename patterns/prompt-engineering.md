# Prompt Engineering as a Project Asset

**Solves:** Writing the same complex prompts from scratch every session, getting inconsistent results because the prompt wording varies, losing prompts that produced great output.

---

## The Problem

Some tasks require carefully crafted prompts: generating copy in a specific voice, producing structured output in an exact format, running a multi-step analysis. When these prompts live only in your head or in a chat history you'll never find again, every session starts from scratch. The output quality varies. You spend time re-figuring out what worked.

---

## The Solution: Store Prompts as Project Files

Treat prompts the same way you treat scripts or tools — version them, name them, put them where future sessions can find them.

### Folder structure

```
project/
└── prompts/
    ├── README.md           ← What's here, how to use
    ├── copy/
    │   ├── hero-headline.md
    │   ├── feature-bullets.md
    │   └── email-subject.md
    ├── analysis/
    │   ├── code-review.md
    │   └── ux-audit.md
    └── generation/
        ├── component-from-spec.md
        └── test-cases.md
```

### What goes in a prompt file

```markdown
# Hero Headline Generator

## Context
Generates 5 hero headline variations for a SaaS product landing page.
Follows the "problem → outcome" structure proven to convert.

## Inputs needed
- Product name: [PRODUCT]
- Core problem it solves: [PROBLEM]
- Target audience: [AUDIENCE]
- Primary outcome/benefit: [OUTCOME]

## Prompt

You are a direct-response copywriter. Generate 5 hero headline variations for [PRODUCT].

Requirements:
- Each headline must name the problem OR the outcome (not both)
- Max 10 words per headline
- No exclamation marks
- No questions
- Tone: confident, direct, no fluff

Format each as:
1. [Headline]
   Hook: [1-sentence subheadline that adds what the headline left out]

Product: [PRODUCT]
Problem: [PROBLEM]
Audience: [AUDIENCE]
Outcome: [OUTCOME]

## Notes

- Best results come from being specific in PROBLEM and OUTCOME
- Version that worked well for Scalyze: "Stop losing leads you already paid for"
- Avoid generic outcomes like "grow your business" — be specific to the product
```

---

## Reference Prompts from CLAUDE.md or MEMORY.md

Tell AI where prompts live so it uses them:

In `CLAUDE.md`:
```markdown
## Tools Available
- `prompts/copy/hero-headline.md` — hero headline generator (5 variations)
- `prompts/analysis/code-review.md` — structured code review checklist
- `prompts/generation/component-from-spec.md` — React component from design spec
```

In any session: **"Use the prompt in `prompts/copy/hero-headline.md`."** AI reads the file, fills in the inputs you provide, and runs the prompt.

---

## Types of Prompts Worth Storing

**High-value, high-reuse candidates:**
- Copy generation with a specific brand voice
- Code generation that must follow exact patterns (component structure, test format)
- Structured analysis that always needs the same output format
- Multi-step tasks where the sequence matters

**Not worth storing:**
- One-off questions
- Simple, short prompts that are obvious
- Prompts that are highly context-specific and won't generalize

---

## Versioning Prompts

When a prompt produces significantly better output after refinement, note the change:

```markdown
## Changelog

- v1 (2026-04-01): Initial version, results were generic
- v2 (2026-04-15): Added "No exclamation marks, no questions" — dramatically cleaner output
- v3 (2026-05-01): Added example of good result — AI now stays closer to that tone
```

---

## Sharing Across Projects

Prompts that generalize beyond a single project belong in a shared location. See [Cross-Project Tool Reuse](cross-project-tool-reuse.md) for the pattern.

Common shareable prompts:
- Brand voice analyzer
- Code review checklist
- UX audit
- SEO meta description generator
- Accessibility audit

---

## Success Metric

- Repeated tasks produce consistent output across sessions
- You never rewrite a complex prompt from memory
- Prompts improve over time (changelog shows iterations)
- New sessions find and use stored prompts instead of improvising

---

## See Also

- [Internal Tools](internal-tools.md) — tools folder with index
- [Cross-Project Tool Reuse](cross-project-tool-reuse.md) — reuse across projects
- [Token Efficiency](token-efficiency.md) — don't re-explain what's already in a file
