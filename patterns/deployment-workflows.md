# Pattern: Deployment Workflows

## The Problem

Deployment details get lost between sessions:
- Server configuration (Apache, nginx, Vercel)
- URL routing setup
- Hidden files that need uploading
- Environment-specific settings

This leads to broken deployments and multiple prompts to debug.

## The Solution

Document deployment specifics in MEMORY.md so AI knows them upfront.

### 1. Server Type in Memory

```markdown
## Deployment

**Server:** Apache (cPanel)
**URL Routing:** .htaccess rewrite rules
**Upload To:** public_html/
```

### 2. Clean URLs Setup

**For Apache/cPanel (.htaccess):**
```apache
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /

  # If request is not a file or directory, add .html
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond %{REQUEST_FILENAME}.html -f
  RewriteRule ^(.*)$ $1.html [L]
</IfModule>
```

**For Vercel (vercel.json):**
```json
{
  "cleanUrls": true,
  "trailingSlash": false
}
```

### 3. Link Format Convention

Once you decide on clean URLs, document the format:

```markdown
## Link Format

- Homepage: `href="/"`
- Pages: `href="services"` (no .html)
- Sections: `href="services#section-id"`
- External: Full URL
```

Then batch-update all files to match.

### 4. Hidden Files Checklist

```markdown
## Hidden Files (Cmd+Shift+. to see)

- `.htaccess` — URL routing (upload with HTML files)
- `.env` — Environment variables (never upload)
```

### 5. Deployment Checklist in Memory

```markdown
## cPanel Deployment Steps

1. Upload all files from `site/` to `public_html/`
2. Include `.htaccess` (hidden file)
3. Preserve `assets/` folder structure
4. Clear browser cache after upload
5. Test: /services, /privacy, /terms
```

## How to Implement

Add to your MEMORY.md:

```markdown
## 🚀 Deployment

**Server:** Apache (cPanel)
**Root:** public_html/
**URL Format:** Clean (no .html)

### Upload Checklist
- [ ] All HTML files
- [ ] .htaccess (hidden)
- [ ] assets/ folder
- [ ] Test clean URLs after upload
```

## Common Issues

### 404 on Clean URLs
- `.htaccess` not uploaded
- mod_rewrite not enabled
- Wrong RewriteBase path

### Links Still Show .html
- Forgot to update all files
- Check with: `grep -rn '\.html' site/*.html | grep 'href='`

### Hidden File Not Visible
- macOS: `Cmd+Shift+.` to toggle hidden files
- cPanel File Manager: Settings → Show Hidden Files

## Measuring Success

You're doing it right if:
- Deployment works first try
- No 404 errors on clean URLs
- AI knows server type without asking
- Hidden files are documented and remembered
