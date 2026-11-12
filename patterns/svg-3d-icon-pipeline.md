# Pattern: SVG 3D Icon & Logo Asset Pipeline

When creating brand icons, logos, or visual marks using SVG with a 3D appearance — and exporting to a full asset package (SVG, PNG multi-size, favicon).

## The Problem

Logo/icon creation involves many iterations (shape, color, dimension), then a tedious export to dozens of sizes and variants. Without structure:
- Iterations sprawl across random files
- Export is manual and inconsistent
- No single source of truth for the final mark
- Re-exporting after tweaks requires repeating everything

## The Solution

A structured iteration → approval → export pipeline using HTML for visual proofing and Puppeteer for batch PNG export.

---

## Phase 1: Shape Exploration

Create an HTML file with multiple variations laid out as `.slide` elements.

```
brand/logos/concepts/{variation-name}.html
```

**Template structure:**
```html
<div class="slide light">  <!-- 1080x1080, bg: {CANVAS_COLOR} -->
  <svg viewBox="0 0 {W} {H}" fill="none">
    <!-- Variation 1 -->
  </svg>
  <span class="label">{VARIATION_LABEL}</span>
</div>
```

- Show 4-8 variations per slide
- Always show on both light (`{CANVAS_COLOR}`) and dark (`{DARK_COLOR}`) backgrounds
- Use the Puppeteer export tool to generate PNGs for review
- User picks a direction → narrow down → iterate

## Phase 2: Color Exploration

Once shape is locked, create a color palette HTML:
```
brand/logos/concepts/{shape}-color-palettes.html
```

- Show 4-6 color schemes
- Each scheme applied to the locked shape
- Both light and dark slides
- User picks or requests custom combo

## Phase 3: 3D Treatment

### SVG 3D Disc (for flat/elongated elements)

Three layers stacked:
```svg
<!-- 1. Side wall (darkest) — path connecting top and bottom ellipse arcs -->
<path d="M {X1},{TOP_Y} A {RX},{RY} 0 0,0 {X2},{TOP_Y} L {X2},{BOT_Y} A {RX},{RY} 0 0,1 {X1},{BOT_Y} Z" fill="{WALL_COLOR}"/>

<!-- 2. Bottom rim (mid-tone) — ellipse at bottom edge -->
<ellipse cx="{CX}" cy="{BOT_Y}" rx="{RX}" ry="{RY}" fill="{RIM_COLOR}"/>

<!-- 3. Top face (lightest) — ellipse at top edge -->
<ellipse cx="{CX}" cy="{TOP_Y}" rx="{RX}" ry="{RY}" fill="{FACE_COLOR}"/>
```

**Color rule:** `FACE_COLOR` (lightest) → `RIM_COLOR` (mid) → `WALL_COLOR` (darkest)

**For rotated/angled elements**, wrap all three layers in a group:
```svg
<g transform="rotate({ANGLE}, {CX}, {CY})">
  <!-- wall path + rim ellipse + face ellipse -->
</g>
```

### SVG 3D Sphere (for circular elements)

Three layers (no bottom cap — element floats):
```svg
<!-- 1. Main body (base tone) -->
<circle cx="{CX}" cy="{CY}" r="{R}" fill="{BODY_COLOR}"/>

<!-- 2. Top highlight (lighter, offset up) -->
<ellipse cx="{CX}" cy="{CY - offset}" rx="{R*0.7}" ry="{R*0.4}" fill="{HIGHLIGHT_COLOR}"/>

<!-- 3. Specular dot (lightest, small, top-left) -->
<circle cx="{CX - offset}" cy="{CY - offset}" r="{R*0.2}" fill="{SPECULAR_COLOR}"/>
```

**Important:** No bottom shadow/cap — sphere floats freely. Drop specular dot at small sizes.

## Phase 4: Proof Sheet

Create a comprehensive HTML with:
1. Large standalone icon
2. Inline lockup (icon embedded in wordmark text)
3. Size test row: Hero ({LARGE}px), Navbar ({MED}px), Social ({SMALL}px), Favicon ({TINY}px)
4. Color legend
5. Both light and dark slides

```
brand/logos/concepts/{final-name}.html → exports to brand/logos/concepts/{final-name}/
```

Use existing Puppeteer export tool:
```bash
cd tools/social-export && node export.js "../../brand/logos/concepts/{FILE}.html" "../../brand/logos/concepts/{FILE}"
```

## Phase 5: Final Asset Export

### Folder Structure
```
brand/logos/exports/
├── svg/
│   ├── icon-light.svg          ← Standalone mark, light bg colors
│   ├── icon-dark.svg           ← Standalone mark, dark bg colors
│   ├── lockup-light.svg        ← Full wordmark + mark, light
│   ├── lockup-dark.svg         ← Full wordmark + mark, dark
│   ├── {element}-light.svg     ← Reusable element (e.g., sphere)
│   └── {element}-dark.svg
├── png/
│   ├── light/
│   │   ├── icon-512.png        ← With background
│   │   ├── icon-256.png
│   │   ├── icon-128.png
│   │   ├── icon-64.png
│   │   ├── icon-32.png
│   │   ├── icon-16.png
│   │   ├── icon-transparent-512.png   ← No background
│   │   ├── icon-transparent-256.png
│   │   ├── icon-transparent-128.png
│   │   ├── lockup.png
│   │   ├── {element}-512.png
│   │   ├── {element}-transparent-256.png
│   │   └── ...
│   └── dark/
│       └── (same structure)
└── favicon/
    ├── favicon.svg
    ├── favicon-48.png
    ├── favicon-32.png
    └── favicon-16.png
```

### Export Script Pattern

Use Puppeteer with system Chrome to render SVGs at exact pixel sizes:

```javascript
const puppeteer = require('{PATH_TO_PUPPETEER}');
const CHROME = '/Applications/Google Chrome.app/Contents/MacOS/Google Chrome';

// For SVG → PNG at specific size:
const svg = fs.readFileSync(svgPath, 'utf8');
const html = `<html><head><style>
  *{margin:0;padding:0;}
  body{width:${size}px;height:${height}px;background:${bgColor};
  display:flex;align-items:center;justify-content:center;overflow:hidden;}
</style></head><body>${adjustedSvg}</body></html>`;

await page.setContent(html, { waitUntil: 'domcontentloaded' });  // NOT networkidle0
await page.setViewport({ width: size, height: height, deviceScaleFactor: 2 });
await page.screenshot({ path: outPath, omitBackground: transparent });
```

**Key settings:**
- `waitUntil: 'domcontentloaded'` — SVG doesn't need network (networkidle0 times out)
- `deviceScaleFactor: 2` — Retina-quality output
- `omitBackground: true` for transparent PNGs, `false` for bg-included
- For lockups with web fonts: use `waitUntil: 'domcontentloaded'` + `document.fonts.ready` + 2s delay

### Lockup SVG Pattern

SVG lockups with text need font references:
```svg
<svg xmlns="http://www.w3.org/2000/svg" width="{W}" height="{H}" viewBox="0 0 {W} {H}">
  <style>@import url('https://fonts.googleapis.com/css2?family={FONT}:wght@{WEIGHT}&amp;display=swap');</style>
  <text x="{X}" y="{Y}" font-family="{FONT}, sans-serif" font-weight="{WEIGHT}" 
        font-size="{SIZE}" letter-spacing="{LS}" fill="{COLOR}">{PREFIX}</text>
  <g transform="translate({MARK_X}, {MARK_Y})">
    <svg width="{MW}" height="{MH}" viewBox="0 0 {VW} {VH}"><!-- mark SVG --></svg>
  </g>
  <text x="{X2}" y="{Y}" ...>{SUFFIX}</text>
</svg>
```

---

## Reusable Elements

Standalone elements (like the sphere) can be reused across the product:
- **Loading indicators** — CSS `@keyframes` rotation on the SVG
- **Status dots** — Swap colors per state
- **App icons** — Transparent PNG at standard sizes
- **Favicon** — Copy dark-variant SVG

## Checklist

- [ ] Shape locked (user approved from variations)
- [ ] Colors locked (user approved palette or custom combo)
- [ ] 3D treatment applied (disc/sphere as appropriate)
- [ ] Proof sheet reviewed on light + dark at all sizes
- [ ] SVGs exported (icon + lockup + elements, light + dark)
- [ ] PNGs exported (6+ sizes, with-bg + transparent, light + dark)
- [ ] Favicon exported (SVG + 48/32/16 PNG)
- [ ] Reusable elements identified and exported standalone
- [ ] Memory updated with final specs (colors, technique, file paths)
