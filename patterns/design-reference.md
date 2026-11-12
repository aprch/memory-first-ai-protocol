# Pattern: Design Reference Analysis

When redesigning a UI or building a new design system, analyze reference sites systematically before implementing.

## The Problem

Random exploration wastes tokens and produces inconsistent designs. Starting from scratch ignores proven patterns.

## The Solution

1. **Pick reference sites** — Ask user for 1-3 sites they admire
2. **Analyze systematically** — Extract structure, not aesthetics
3. **Document patterns** — Create reusable CSS components
4. **Plan before implementing** — Use plan mode for major redesigns

## Reference Analysis Template

When analyzing a site, extract:

```markdown
## Site: [example.com]

### Section Structure (in order)
1. Hero — [what it does]
2. [Section 2] — [purpose]
...

### Design Patterns
- Typography: [scale, weights, families]
- Colors: [system used]
- Spacing: [padding/margins]
- Components: [cards, buttons, etc.]

### What Makes It Clean
- [Key insight 1]
- [Key insight 2]
- [Key insight 3]
```

## CSS Component Checklist

Common patterns to extract:

```css
/* Highlighted words in headlines */
.highlight {
  background: linear-gradient(135deg, #primary, #accent);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* Tabs component */
.tabs { display: flex; gap: 4px; background: #bg; padding: 6px; border-radius: 12px; }
.tab-btn { ... }
.tab-btn.active { ... }
.tab-content { display: none; }
.tab-content.active { display: block; }

/* Comparison cards (before/after) */
.comparison-card { padding: 32px; border-radius: 16px; }
.comparison-card.before { background: #error-light; border: 1px solid #error; }
.comparison-card.after { background: #success-light; border: 1px solid #success; }

/* Stats with large numbers */
.stat-number {
  font-size: clamp(2.5rem, 5vw, 4rem);
  font-weight: 900;
  background: linear-gradient(...);
  -webkit-background-clip: text;
}
```

## Plan Mode for Redesigns

Use plan mode when:
- Reducing section count (e.g., 16 → 7)
- Changing layout patterns significantly
- Adding new CSS component systems

Plan should include:
1. New section structure (table)
2. ASCII wireframes for key sections
3. New CSS components needed
4. What to remove vs. keep
5. Implementation order

## Example: Services Agency Site Redesign

**Reference:** a SaaS marketing site with a strong hero and comparison layout

**Extracted patterns:**
- Hero with tabbed service preview
- Before/after comparison layout
- Stats grid with gradient numbers
- FAQ accordion (kept existing)
- Clean white nav (not colored)

**Result:** 16 sections → 7 sections

**Time saved:** Systematic analysis prevented random experimentation.

---

## Memory Update

After a redesign, update memory with:
- New section structure
- New CSS classes added
- Design patterns used
- Reference site (for future consistency)

## Related Patterns

- `token-efficiency.md` — Don't re-explore; use analysis results
- `session-continuity.md` — Carry design decisions across sessions
- `working-scope.md` — Major redesigns need plan mode
