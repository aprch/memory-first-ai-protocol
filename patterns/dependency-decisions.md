# Dependency Decisions

**Solves:** AI suggests replacing a library you already chose, recommends a package you already evaluated and rejected, or doesn't know why you're using one tool instead of a more obvious alternative.

---

## The Problem

Dependency decisions often happen once — early in a project — and the reasoning is never written down. Three weeks later, a new session sees you using `react-hook-form` and suggests migrating to `formik`. Or it tries to add `moment.js` when you've already committed to `date-fns`. Or it recommends `axios` when you're using the native `fetch` API intentionally.

These suggestions waste time, create discussion that already happened, and sometimes produce churn (switching a library mid-project because AI was persuasive).

---

## The Solution: Document Dependency Decisions in MEMORY.md

Create a **Dependency Decisions** section in `memory/MEMORY.md` or a dedicated `memory/dependencies.md` topic file.

### Format

```markdown
## Dependency Decisions

| Package | Status | Reason |
|---|---|---|
| react-hook-form | ✅ Chosen | Lighter than Formik, better TS support, uncontrolled inputs |
| formik | ❌ Rejected | Heavier, more re-renders, evaluated 2026-04-01 |
| date-fns | ✅ Chosen | Tree-shakeable, no side effects, modern API |
| moment.js | ❌ Rejected | 300KB, deprecated, mutable |
| axios | ❌ Rejected | Using native fetch + custom wrapper — no extra dep needed |
| zustand | ✅ Chosen | Simpler than Redux, no boilerplate, works well with React Query |
| redux | ❌ Rejected | Overkill for this project's scope |
| prisma | ✅ Chosen | Type-safe queries, good migration tooling, team knows it |
| drizzle | ⚠️ Deferred | Interesting but not stable enough at project start (2026-03-15) |
```

### Status keys
- ✅ **Chosen** — using this, don't suggest alternatives
- ❌ **Rejected** — evaluated and decided against; don't suggest again unless asked
- ⚠️ **Deferred** — not now, maybe later; include the date so future sessions can reassess
- 🔄 **Under evaluation** — actively comparing, decision pending

---

## When to Add an Entry

Add a row any time you:
- Install a new package (✅)
- Actively consider and reject a package (❌)
- See AI suggest something you've already thought about and decided against (❌ with note)
- Use a version-pinned package where the reason matters (e.g., locked to v4 for a specific API)

---

## Version Pinning

If a specific version matters, note it:

```markdown
## Pinned Versions

| Package | Version | Reason |
|---|---|---|
| next | 14.2.x | Not upgrading to 15 until App Router hydration bug is fixed |
| @stripe/stripe-js | 3.x | v4 changes webhook signature format — migration planned for Q3 |
| node | 20.x | LTS — team standardized on this |
```

---

## In CLAUDE.md

If certain packages are foundational to the project, add them to CLAUDE.md so AI knows from the start of every session:

```markdown
## Stack (Non-negotiable)

- State: Zustand + React Query (not Redux, not Context for server state)
- Forms: React Hook Form (not Formik)
- Dates: date-fns (not moment.js, not dayjs)
- HTTP: native fetch (not axios)
- Styling: Tailwind (no CSS-in-JS, no CSS modules)
```

This prevents AI from ever suggesting an alternative for core decisions.

---

## What Not to Document

Don't add every transitive dependency. Focus on:
- Packages you made a deliberate choice about
- Packages where an obvious alternative exists that you've evaluated
- Packages where the version matters

You don't need an entry for `eslint`, `prettier`, `typescript`, etc. unless you made non-standard choices.

---

## Success Metric

- AI never suggests replacing a library you've already chosen
- Rejected packages don't resurface in recommendations
- Dependency discussions from week 1 don't repeat in week 8
- New team members or sessions understand the tech stack rationale immediately

---

## See Also

- [Learning Loops](learning-loops.md) — corrections become rules immediately
- [Context Management](context-management.md) — index over dump; topic files for depth
- [Token Efficiency](token-efficiency.md) — keep MEMORY.md lean; move to topic files when long
