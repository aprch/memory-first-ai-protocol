# Pattern: Logo-First Site Development

## The Problem

Sites get built without logos ready. This blocks:
- Navbar (placeholder text or missing image)
- Footer (broken or inconsistent logo)
- Favicon (generic or missing)
- OG/social sharing images (no brand mark)
- Auth pages (no visual identity)
- Theme screenshots (unprofessional preview)

The logo becomes a bottleneck that causes rework across every page.

## The Solution

Create the logo BEFORE building any site pages. Deliver it as a complete asset package that covers every use case upfront.

---

## Required Logo Assets (Minimum Viable)

| Asset | Size/Format | Where It's Used |
|-------|-------------|----------------|
| SVG source (light variant) | Vector | Navbar, auth pages, light backgrounds |
| SVG source (dark variant) | Vector | Footer, dark sections |
| Favicon | 32x32 PNG + SVG | Browser tab |
| Apple touch icon | 180x180 PNG | iOS bookmark |
| OG image | 1200x630 PNG | Social sharing |
| Theme screenshot | 1200x900 PNG | WordPress theme preview |
| Multi-size PNGs | 16–1024px | README, docs, fallbacks |

## Why Inline SVG Over Image Files

```
❌ <img src="/logo.png" alt="Logo">
✅ <svg viewBox="0 0 40 40">...</svg>
```

**Benefits:**
- Zero network requests (no 404 risk)
- Instant render (no image load delay)
- Themeable with CSS/props (change colors per context)
- Crisp at any resolution (vector)
- Smaller payload (2KB SVG vs 50KB+ PNG)
- Works in email, dark mode, print

**When to use `<img>` instead:**
- Complex photographic logos (rare)
- CMS-managed logos where users upload their own
- Third-party logo walls (customer logos, partner logos)

## The Pipeline

### Phase 1: Shape Exploration
Create 4-8 SVG variations in an HTML proof sheet:
```
brand/logos/concepts/exploration.html
```
Show on both light and dark backgrounds. User picks direction.

### Phase 2: Refinement
Narrow to 1-2 candidates. Iterate on proportions, stroke width, color.

### Phase 3: Export Package
Use Puppeteer or similar to batch-export:
```
brand/logos/
├── mark-light.svg          ← Light background version
├── mark-dark.svg           ← Dark background version
├── favicon.svg             ← Browser tab
├── favicon-32.png
├── apple-touch-icon.png    ← 180x180
├── og-image.png            ← 1200x630
├── png/
│   ├── mark-16.png
│   ├── mark-32.png
│   ├── mark-64.png
│   ├── mark-128.png
│   ├── mark-256.png
│   ├── mark-512.png
│   └── mark-1024.png
└── export-log.md           ← What was exported, when, from what source
```

### Phase 4: Embed
Copy the `<svg>` markup directly into:
- Navbar component (light variant)
- Footer component (dark variant)
- Auth/login pages (mark only, light variant)
- `index.html` favicon link

## Platform-Specific Notes

### React / Vite
```jsx
// Inline in component
<svg viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
  {/* ... */}
</svg>
```

### WordPress Block Theme
```html
<!-- parts/header.html -->
<!-- wp:site-title /-->
<!-- Or for fixed branding: -->
<!-- wp:html -->
<a href="/" class="custom-logo-link">
  <svg viewBox="0 0 40 40">...</svg>
</a>
<!-- /wp:html -->
```

Also register `custom-logo` support in `functions.php` for user customization.

### Next.js
```tsx
// components/Logo.tsx — single source, import everywhere
export function Logo({ variant = 'light' }: { variant?: 'light' | 'dark' }) {
  return <svg>...</svg>;
}
```

## How to Implement

Add to your CLAUDE.md:
```markdown
## Logo Requirements
- Logo assets MUST exist before building any page
- Use inline SVG for navbar, footer, auth (not <img>)
- Maintain light + dark variants
- Logo package location: brand/logos/
```

## Common Mistakes

1. **Using PNG in navbar** — Blurry on retina, extra request, can't theme
2. **Different logos per page** — Inconsistent brand. Use ONE SVG source.
3. **Forgetting dark variant** — Footer on dark bg with dark logo = invisible
4. **No favicon** — Looks unprofessional, hurts recognition
5. **No OG image** — Social shares show blank/random image
6. **Building pages first** — Then retrofitting logo everywhere = rework

## Measuring Success

You're doing it right if:
- Logo renders on first paint (no flash/pop-in)
- Same mark appears identically across navbar, footer, auth
- Social shares show branded OG image
- Favicon matches the mark
- Switching color themes doesn't break the logo

## Related Patterns

- `svg-3d-icon-pipeline.md` — Full SVG creation and export workflow
- `asset-organization.md` — Organizing exported files
- `design-reference.md` — Analyzing reference sites before building
