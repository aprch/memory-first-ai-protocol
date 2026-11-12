# Pattern: Site File Architecture

Standard file structures for marketing/product sites across platforms. Use these as starting scaffolds instead of inventing structure per project.

## The Problem

Every new site project starts with "where do I put things?" decisions. Without a standard, files end up disorganized, logos go in random places, and deployment configs are inconsistent.

## The Solution

Standardized file structures for each platform, with a mapping table so knowledge transfers between them.

---

## React + Vite + Tailwind (SPA Marketing Site)

```
site/
├── index.html              ← SEO meta, OG tags, font <link>, favicon
├── package.json
├── vite.config.js
├── tailwind.config.js      ← Brand tokens (colors, fonts, spacing)
├── postcss.config.js
├── vercel.json             ← Deploy: framework, rewrites, build config
├── .gitignore              ← node_modules, dist, .DS_Store, .env, *.bak
│
├── public/                 ← Static (copied as-is to dist/)
│   ├── favicon.svg
│   ├── og-image.png        ← 1200x630
│   └── robots.txt
│
├── src/
│   ├── main.jsx            ← React entry (createRoot)
│   ├── App.jsx             ← Pages + sections + routing
│   ├── index.css           ← @tailwind directives, :root CSS vars
│   └── components/         ← Reusable pieces
│       ├── Header.jsx
│       ├── Footer.jsx
│       └── ...
│
└── assets/                 ← Design assets (may or may not be imported)
    ├── fonts/
    ├── icons/
    └── images/
```

### Key Decisions
- **Logos:** Inline SVG in components, NOT in public/ as images
- **Fonts:** Single `<link>` in index.html (Google Fonts) or local in assets/fonts/
- **Colors:** Defined in tailwind.config.js, mirrored as :root vars in index.css
- **Deploy:** vercel.json with `"framework": "vite"` and SPA rewrite rule

---

## WordPress Block Theme (FSE)

```
theme-name/
├── style.css               ← Theme header (name, version, author) — required
├── theme.json              ← Design tokens (colors, fonts, spacing) — the "tailwind.config"
├── functions.php           ← Enqueue styles, register features, add_theme_support
├── index.php               ← Required PHP fallback (can be empty)
├── screenshot.png          ← Theme preview 1200x900 — required by marketplaces
├── readme.txt              ← Changelog, credits, resources
├── .gitignore
│
├── templates/              ← Full page layouts (block HTML)
│   ├── index.html          ← Blog/archive fallback
│   ├── front-page.html     ← Homepage
│   ├── page.html           ← Generic page
│   ├── single.html         ← Single post
│   ├── 404.html            ← Not found
│   ├── search.html         ← Search results
│   └── ...
│
├── parts/                  ← Reusable template parts
│   ├── header.html         ← Site header with nav + logo
│   └── footer.html         ← Site footer with logo
│
├── patterns/               ← Block patterns (reusable sections)
│   ├── hero.php            ← Hero section
│   ├── features.php        ← Features grid
│   ├── pricing.php         ← Pricing table
│   ├── testimonials.php    ← Social proof
│   ├── faq.php             ← FAQ accordion
│   └── cta.php             ← Call to action
│
├── assets/
│   ├── css/
│   │   ├── main.css        ← Custom styles beyond theme.json
│   │   └── editor.css      ← Editor-side style mirror
│   ├── fonts/              ← Local .woff2 fonts (GDPR required)
│   │   ├── inter-regular.woff2
│   │   └── inter-semibold.woff2
│   └── images/
│       └── dashboard-placeholder.svg
│
└── languages/
    └── theme-name.pot      ← Translation strings (required for translation-ready tag)
```

### Key Decisions
- **Logos:** Inline SVG in parts/header.html and parts/footer.html + register custom-logo support
- **Fonts:** Local .woff2 via theme.json fontFace — NEVER Google Fonts CDN (GDPR)
- **Colors:** Defined in theme.json palette — available in block editor and frontend
- **Required files:** style.css, index.php, theme.json, screenshot.png

---

## Cross-Platform Mapping

| Concept | React + Vite | WordPress FSE |
|---------|-------------|---------------|
| **Design tokens** | `tailwind.config.js` | `theme.json` |
| **Page layouts** | `App.jsx` sections | `templates/*.html` |
| **Reusable sections** | `components/*.jsx` | `patterns/*.php` |
| **Header/Footer** | Inline in App.jsx or components/ | `parts/header.html`, `parts/footer.html` |
| **Custom CSS** | `src/index.css` | `assets/css/main.css` |
| **CSS variables** | `:root` in index.css | `theme.json` custom properties |
| **Font loading** | `<link>` in index.html | `fontFace` in theme.json (local) |
| **SEO meta** | `<meta>` in index.html | `functions.php` (wp_head) or SEO plugin |
| **Deploy config** | `vercel.json` | Hosting-specific (or none) |
| **Static assets** | `public/` | `assets/images/` |
| **Logo** | Inline SVG in JSX | Inline SVG in block HTML |
| **Build** | `npm run build` → dist/ | Zip → upload to marketplace |

---

## Scaffold Commands

### React + Vite
```bash
npm create vite@latest site -- --template react
cd site
npm i tailwindcss @tailwindcss/postcss autoprefixer postcss framer-motion lucide-react
# Then add tailwind.config.js, postcss.config.js, vercel.json, .gitignore
```

### WordPress Theme
```bash
mkdir theme-name && cd theme-name
mkdir -p templates parts patterns assets/{css,fonts,images} languages
touch style.css theme.json functions.php index.php readme.txt .gitignore
# Then populate theme header in style.css, tokens in theme.json
```

---

## Pre-Publish Checklist (Both Platforms)

- [ ] Logo inline SVG in header + footer (light + dark variants)
- [ ] Favicon set (SVG or 32x32 PNG)
- [ ] OG image exists (1200x630)
- [ ] SEO meta tags present
- [ ] Fonts loaded from single source
- [ ] Colors use tokens (not hardcoded hex)
- [ ] Mobile responsive (375px tested)
- [ ] Hamburger menu on mobile
- [ ] .gitignore covers build artifacts
- [ ] Production build passes clean
- [ ] No dead/unused assets

## Related Patterns

- `logo-first-site-development.md` — Logo must exist before building pages
- `deployment-workflows.md` — Server config and deploy steps
- `design-reference.md` — Analyze reference sites before building
- `asset-organization.md` — Keep exports and outputs organized
