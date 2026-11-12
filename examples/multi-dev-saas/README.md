# Example: Multi-Developer SaaS Project

This shows how Memory-First Protocol works when **multiple developers and multiple AI sessions** work on the same codebase — not just a solo dev.

The solo-dev pattern (see `web-project/`) keeps memory in `memory/MEMORY.md` for one person.
This pattern adds **shared memory artifacts** that any developer, any AI session, any time can pick up without context.

---

## The Problem This Solves

On a solo project, memory lives in `memory/MEMORY.md` — personal, fast, yours.

On a team project:
- Dev A fixes something in a session. Dev B's AI session doesn't know.
- You correct the AI. Your teammate's AI makes the same mistake next week.
- You document a UI decision. Next AI session starts fresh, ignores it.
- Different AI sessions contradict each other's output.

**Memory-First Protocol extends to teams by promoting certain memory into shared artifacts.**

---

## Key Insight: Two Levels of Memory

```
PERSONAL memory (your AI reads this):
  memory/MEMORY.md           ← your current context, session notes
  memory/project-state.md    ← what's done, what's next

SHARED memory (every AI reads this, every dev sees this):
  COLLABORATE.md             ← rules for working on this repo
  UI_FIXES.md                ← structured list of known issues to work through
  CLAUDE.md                  ← AI-specific rules for this project
```

The shared files are committed to the repo. Any AI session that opens this project immediately knows the rules, the open work, and what not to touch.

---

## File Structure

```
your-saas-project/
├── CLAUDE.md                  ← AI rules (stack, branch rules, scope)
├── COLLABORATE.md             ← Team rules (branches, how to pick up work, V1 scope)
├── UI_FIXES.md                ← Shared issue tracker (AI works through this top to bottom)
├── memory/
│   ├── MEMORY.md              ← Personal index (your AI reads this)
│   └── project-state.md      ← Active work log, decisions, key files
└── src/
    └── ...
```

---

## COLLABORATE.md

Replaces the "read the README and figure it out" problem. Written for both humans and AI.

```markdown
# Project — Collaboration Guide

## Branches
- `main` — stable, auto-deploys from here
- `development` — all active work goes here
- Never push code directly to `main` — docs/config files only

## Picking Up Work
1. Read `BRIEF.md` first — defines V1 scope (what's in, what's out)
2. Read `UI_FRAMEWORK.md` before touching any screen
3. Check `UI_FIXES.md` for open issues — work top to bottom, check boxes when done
4. All code changes go to `development` branch

## V1 Scope
Admin console · Customer-facing app · Auth · Billing · Support.
Reporting and integrations exist but are post-V1. Do not add AI features or third-party tooling.

## Stack
Next.js App Router · PostgreSQL · React Query · Tailwind · JWT auth
```

**Why this works:** Any AI session reading this immediately knows the branch rules, scope boundary, and where to find open work. No explanation needed in chat.

---

## UI_FIXES.md — The Shared Issue Tracker

Instead of tracking UI issues in Linear, Jira, or a spreadsheet that AI can't read, put them in a markdown file in the repo.

```markdown
# UI Fixes Tracker

Work through these top to bottom. Check the box when done.

---

## Admin Console

### Navigation & Layout
- [x] Move user identity to bottom-left sidebar
- [ ] Remove internal "Admin Console" label visible to end users
- [ ] Rename "Tools" to "Utilities" in sidebar
- [ ] Move Settings to bottom of sidebar

### Loading States
- [ ] Add skeleton loaders to slow-loading pages (Users, Reports, Audit Log)
- [ ] Differentiate: loading (skeleton) · empty (CTA) · error (message + retry)

---

## Customer App

### Navigation & Layout
- [ ] User identity block missing from sidebar — currently in top-right header only
- [ ] Settings not visible in mobile nav
- [ ] Role-gated sections visible to all users — should hide when permission missing

### Forms
- [ ] No inline validation on profile edit form
- [ ] No feedback on submit — silent failures

---

## Reporting

*(Add issues here as you test)*
```

**Why this works:**
- AI opens the project, reads `UI_FIXES.md`, knows exactly what to work on
- No "what should I work on today?" — top unchecked box is the answer
- Human tests → finds issue → adds to file → next AI session picks it up
- Works section by section — manageable scope

---

## memory/MEMORY.md (Personal)

The personal layer works the same as solo-dev pattern. It's yours.

```markdown
# Memory Index

## Project Status

**Current Work:** Customer App UI fixes — identity block, role-gating
**Last Session:** [DATE] — Customer App section in UI_FIXES.md populated and pushed

---

## Active Work

| Task | Status | Notes |
|------|--------|-------|
| Identity block in sidebar | Pending | Move from header to sidebar bottom |
| Reporting UI audit | Pending | Test, then populate UI_FIXES.md section |
| Create account flow | Local only | Code fix done, not committed yet |

---

## Key Decisions

- [DATE]: Docs go to main, code goes to development — no exceptions
- [DATE]: UI audit order: Admin Console → Customer App → Reporting

---

## Locked Preferences

- Never push code to main (docs/config only)
- UI audit follows console order top to bottom
- Update UI_FIXES.md checkbox before finishing each session

---

## File Map

- Sidebar nav: `src/components/layout/sidebar-nav.tsx`
- App shell: `src/components/layout/app-shell.tsx`
- Customer layout: `src/app/[orgId]/layout.tsx`

---

## Session Notes

**[DATE]:** Admin Console + Customer App sections in UI_FIXES.md complete. Pushed to main.
```

---

## How It Plays Out in Practice

### Any AI session, zero context needed

```
You: "Continue"

AI: *Reads CLAUDE.md — knows stack, scope, branch rules*
AI: *Reads COLLABORATE.md — knows what not to do*
AI: *Reads memory/MEMORY.md — knows current task*
AI: "Continuing Customer App UI work. Next unchecked item in UI_FIXES.md is the
    identity block — currently in top-right header, needs to move to
    bottom-left sidebar. Should I start there?"
```

### Teammate's AI session picks up where you left off

Your teammate opens the same repo, different machine:

```
Teammate's AI: *Reads COLLABORATE.md — understands branch rules, scope*
Teammate's AI: *Reads UI_FIXES.md — sees checked boxes, knows what's done*
Teammate's AI: "Admin Console nav fixes are done. Customer App is in progress.
               First unchecked item is the identity block — ready to work on that."
```

No handoff call. No "let me catch you up." The repo is the memory.

### Human tests, finds bug, AI picks it up next session

```
Human: *Tests Reporting section, finds filter panel broken on mobile*
Human: *Adds to UI_FIXES.md: "- [ ] Filter panel breaks layout on mobile"*

Next AI session:
AI: *Reads UI_FIXES.md*
AI: "Found new item in Reporting section — filter panel mobile issue.
    Want me to start there?"
```

---

## What Makes This Different from Issue Trackers

| | Linear/Jira | UI_FIXES.md |
|---|---|---|
| AI can read it | No | Yes |
| Works offline | No | Yes |
| In the repo | No | Yes |
| Near the code | No | Yes |
| Team can edit | Yes | Yes |
| Overhead | High | Low |

This isn't a replacement for proper project management on large teams. It's a low-overhead alternative for small teams where the AI needs to know the work.

---

## The Pattern in One Sentence

> Move the work into the repo, and any AI session — yours, your teammate's, tomorrow's — immediately knows what to do.
