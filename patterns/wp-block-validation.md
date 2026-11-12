# WordPress Block Validation — Avoiding "Attempt Recovery" Errors

**Context:** FSE (Full Site Editing) block theme development. Applies to any static block HTML written in theme templates or patterns.

---

## The Core Rule

WordPress validates static blocks by running the block's JS `save()` function and comparing the output byte-for-byte with the stored HTML. Any mismatch triggers "attempt recovery" in the Site Editor.

**Dynamic blocks** (post-title, post-content, query, post-template, post-featured-image, etc.) are server-rendered — they are never validated client-side. Only worry about static blocks.

---

## Step 0 — Always Clear the Database First

When "attempt recovery" appears and gets saved, WordPress writes the broken HTML to `wp_posts` as a `wp_template` entry. That DB version permanently overrides theme files.

**Before debugging any markup, clear the DB override:**
```bash
bash tools/db-reset.sh
```
Editing theme PHP files alone will never fix the error while a DB override exists.

---

## Writing Valid Block HTML — The Rules

### 1. Inline styles must match JSON style attributes

If a block has `style.spacing.padding` or `style.spacing.margin` in its block comment JSON, the HTML element MUST have the corresponding `style="..."` attribute.

```html
<!-- WRONG -->
<!-- wp:group {"style":{"spacing":{"padding":{"top":"var:preset|spacing|20"}}}} -->
<div class="wp-block-group">

<!-- RIGHT -->
<!-- wp:group {"style":{"spacing":{"padding":{"top":"var:preset|spacing|20"}}}} -->
<div class="wp-block-group" style="padding-top:var(--wp--preset--spacing--20)">
```

`var:preset|spacing|x` in JSON → `var(--wp--preset--spacing--x)` in HTML.

### 2. Prefer preset slugs over style objects for colors/fonts

```html
<!-- WRONG — needs inline style in HTML -->
{"style":{"color":{"text":"var:preset|color|primary"}}}

<!-- RIGHT — generates class only, no inline style needed -->
{"textColor":"primary"}  →  has-primary-color has-text-color
```

### 3. Every `<ul>` and `<ol>` needs `wp-block-list`

```html
<!-- WRONG -->
<ul class="has-sm-font-size">

<!-- RIGHT -->
<ul class="wp-block-list has-sm-font-size">
```

`core/list-item` has `className:false` — `<li>` gets no automatic class.

### 4. No `<span>` inside heading or paragraph blocks

```html
<!-- WRONG -->
<h4 class="...">$49 <span class="period">/ month</span></h4>

<!-- RIGHT -->
<h4 class="...">$49 / month</h4>  <!-- style in CSS via className -->
```

### 5. Button block class order

**Wrapper div:** `wp-block-button {className} has-custom-width wp-block-button__width-{N}`
- className (e.g. `is-style-outline`) comes BEFORE width classes

**Anchor:** `wp-block-button__link has-{bgColor}-background-color has-{textColor}-color has-text-color has-background wp-element-button`
- backgroundColor slug comes FIRST, then textColor slug

### 6. `blockGap` in constrained layout — no inline CSS needed

In `constrained` layout groups, `blockGap` in JSON does NOT generate an inline CSS property. Safe to include without matching HTML.

### 7. Move visual effects to CSS

Never put these in block JSON `style` objects:
- `"display":"inline-flex"`
- `"background":"linear-gradient(...)"`

Use `className` and define in `assets/css/main.css`.

---

## Debugging Checklist

When "attempt recovery" appears:
1. Run `db-reset.sh` — clear DB override first
2. Identify the specific failing block (the recovery dialog names it)
3. Check: does every static block's HTML have the inline styles its JSON declares?
4. Check: is `wp-block-list` on every `<ul>`/`<ol>`?
5. Check: are there `<span>` tags inside headings or paragraphs?
6. Check: do button wrapper and anchor classes follow the correct order?
7. Verify in browser — never use Claude's preview for WordPress local dev
