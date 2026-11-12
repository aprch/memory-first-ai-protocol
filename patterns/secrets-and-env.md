# Secrets and Environment Variables

**Solves:** Accidentally committing API keys to git, AI reading or logging secrets, `.env` values leaking into memory files, confusion about which credentials go where.

---

## The Problem

Memory files are plain text. MEMORY.md and CLAUDE.md sit in your project folder. If you paste an API key into either of these, it ends up in git history. AI tools also sometimes echo file contents back in responses — including any secrets that were in those files.

Additionally, AI sessions don't retain environment state between runs, so you end up re-explaining "use the STRIPE_SECRET_KEY env var" every session.

---

## The Rules

### Never put real secrets in memory files

Wrong:
```
## API Keys
- Stripe: sk_live_abc123xyz...
- OpenAI: sk-proj-...
```

Right:
```
## Environment Variables
- STRIPE_SECRET_KEY — live key, in .env.local (not committed)
- OPENAI_API_KEY — in .env.local
- DATABASE_URL — see 1Password "Project DB" entry
```

Reference names, not values. AI knows to use `process.env.STRIPE_SECRET_KEY` — it doesn't need to see the key.

### .gitignore before first commit

Add these before committing anything:
```
.env
.env.local
.env.production
.env.*.local
```

Check your `.gitignore` exists and covers all env files before `git init` + first push.

### Document where secrets live, not what they are

In MEMORY.md or CLAUDE.md, note:
- Which env vars the project uses
- Where to find the actual values (1Password vault name, AWS Secrets Manager path, team member to ask)
- Which are required vs. optional

```markdown
## Environment Variables
| Variable | Required | Where to find |
|---|---|---|
| DATABASE_URL | Yes | 1Password → "MyApp DB" |
| STRIPE_SECRET_KEY | Yes | Stripe dashboard → Developers |
| SENDGRID_API_KEY | Yes | Team lead has this |
| NEXT_PUBLIC_SITE_URL | Yes | Set to http://localhost:3000 locally |
| ANALYTICS_ID | No | Google Analytics dashboard |
```

### Provide a .env.example file

Commit a `.env.example` with all variable names but no values:
```
DATABASE_URL=
STRIPE_SECRET_KEY=
SENDGRID_API_KEY=
NEXT_PUBLIC_SITE_URL=http://localhost:3000
```

This tells future sessions (and team members) exactly what's needed without exposing anything.

---

## When AI Asks for Credentials

AI should never need the actual value of a secret. If a session asks "what is your Stripe key?", that's a sign the request is wrong. Instead:

- For code: AI writes `process.env.STRIPE_SECRET_KEY` and you verify it runs in your terminal
- For testing: You run the command yourself; AI reads the output
- For config: Point AI to the `.env.example` for the list of variables

---

## Git History Check

If you've already accidentally committed a secret:

1. Rotate the key immediately (don't wait — assume it's compromised)
2. Remove from history: `git filter-branch` or `git-filter-repo` (nuclear option, rewrites history)
3. Force push to remote
4. Update `.gitignore` so it doesn't happen again

GitHub will also alert you if it detects known secret patterns in commits.

---

## Success Metric

- No real values in MEMORY.md, CLAUDE.md, or any committed file
- `.env` and variants are in `.gitignore` from the first commit
- `.env.example` exists with all variable names
- AI knows what env vars to use by name, not by value

---

## See Also

- [File Safety](file-safety.md) — archive over delete, user approves destructive actions
- [Working Scope](working-scope.md) — AI stays in project folder and doesn't read `~/.ssh`, `~/.aws`, etc.
