# Example: Web Project

This shows how the Memory-First AI Protocol looks in a real web project.

## File Structure

```
my-web-app/
├── CLAUDE.md                    ← Project rules
├── memory/
│   ├── MEMORY.md                ← Active context
│   └── api-decisions.md         ← Detailed API notes
├── src/
│   ├── components/
│   ├── pages/
│   └── utils/
├── tests/
└── package.json
```

## CLAUDE.md

```markdown
# CLAUDE.md — My Web App

## Project Overview

**Name:** My Web App
**Stack:** Next.js 14 + TypeScript + Tailwind + Prisma
**Status:** Active development

---

## Session Protocol

1. Read memory/MEMORY.md for context
2. Check existing code before writing new
3. Follow conventions below
4. Update memory after significant work

---

## Code Conventions

- TypeScript strict mode, no `any`
- Functional components, hooks only
- Tailwind for styling, no CSS files
- Server components by default, 'use client' only when needed
- Zod for validation, no manual type checking

---

## Design Rules

- No emojis as icons — use Lucide React
- All clickable elements: cursor-pointer
- Mobile-first: test 375px → 768px → 1024px
- Min 44×44px touch targets
- Focus states visible

---

## Pre-Delivery

- [ ] TypeScript compiles (npm run build)
- [ ] Tests pass (npm test)
- [ ] No console.log
- [ ] Mobile responsive

---

## Token Efficiency

- Use bash for bulk operations
- Don't create new files — edit existing
- MEMORY.md under 200 lines

---

**Updated:** 2024-01-15
```

## memory/MEMORY.md

```markdown
# Memory Index

## Project Status

**Phase:** MVP Development
**Last Session:** 2024-01-15 — Finished user auth, starting dashboard

---

## Active Work

| Task | Status | Notes |
|------|--------|-------|
| User dashboard | In Progress | Layout done, need widgets |
| API rate limiting | Pending | After dashboard |
| Email notifications | Blocked | Waiting on SMTP config |

---

## Key Decisions

- **2024-01-10:** Using NextAuth for auth (not custom JWT)
- **2024-01-08:** Prisma over raw SQL (type safety)
- **2024-01-05:** Tailwind + shadcn/ui component library

---

## Locked Preferences

- No `any` type — use proper types or `unknown`
- Server components default — only 'use client' when needed
- Lucide icons only — no Font Awesome, no emojis
- Error messages must be user-friendly, no stack traces

---

## Project Map

```
src/
├── app/               ← Next.js app router
│   ├── api/           ← API routes
│   ├── (auth)/        ← Auth pages (login, register)
│   └── dashboard/     ← Protected dashboard
├── components/
│   ├── ui/            ← shadcn components
│   └── custom/        ← Our components
├── lib/
│   ├── db.ts          ← Prisma client
│   └── auth.ts        ← NextAuth config
└── types/             ← TypeScript types
```

---

## Topic Files

- `memory/api-decisions.md` — API structure decisions

---

## Session Notes

**2024-01-15:** Auth complete. Dashboard layout done. Next: widget components.
**2024-01-14:** Set up Prisma, created User model. NextAuth integrated.
**2024-01-13:** Project init. Chose stack. Set up repo.

---

**Updated:** 2024-01-15
```

## memory/api-decisions.md

```markdown
# API Decisions

Detailed notes on API structure. Referenced from MEMORY.md.

## Endpoint Structure

```
/api/v1/
├── /auth          ← NextAuth handles (don't touch)
├── /users
│   ├── GET /me    ← Current user profile
│   └── PATCH /me  ← Update profile
├── /dashboard
│   └── GET /stats ← Dashboard statistics
└── /webhooks      ← External integrations
```

## Auth Approach

Using NextAuth with:
- Credentials provider (email/password)
- Google OAuth (optional)
- JWT strategy (not database sessions)

Tokens expire in 7 days. Refresh handled by NextAuth.

## Rate Limiting

Not implemented yet. Plan:
- Use upstash/ratelimit
- 100 requests/minute for authenticated
- 20 requests/minute for anonymous

## Error Format

All API errors return:
```json
{
  "error": "Human readable message",
  "code": "MACHINE_READABLE_CODE"
}
```

Never expose stack traces or internal details.

---

**Updated:** 2024-01-12
```

---

## How This Works in Practice

### Session 1: Start Dashboard

```
You: "Continue"

AI: *Reads CLAUDE.md and memory/MEMORY.md*
AI: "Continuing from dashboard work. Layout is done, starting on widgets.
    Which widget should we build first — stats overview or recent activity?"
```

### Session 2: After Correction

```
You: "Don't use Font Awesome, we use Lucide."

AI: *Fixes code to use Lucide*
AI: *Updates memory/MEMORY.md Locked Preferences:
    "Lucide icons only — no Font Awesome, no emojis"*
AI: "Fixed. I've added this to locked preferences so I won't use Font Awesome again."
```

### Session 3: Remembers Everything

```
You: "Add an icon to the dashboard header"

AI: *Uses Lucide without being told*
AI: *Follows TypeScript conventions*
AI: *Makes it responsive per design rules*
```

No re-explaining. No repeated mistakes. Context builds over time.
