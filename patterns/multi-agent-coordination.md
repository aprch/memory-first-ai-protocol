# Multi-Agent Coordination

**Solves:** Running multiple AI sessions simultaneously on the same project — without them conflicting, duplicating work, or overwriting each other's changes.

---

## The Problem

Large projects sometimes benefit from parallel AI sessions: one working on backend, one on frontend, one reviewing docs. But without coordination:

- Both sessions might edit the same file simultaneously
- Session A makes a decision; Session B doesn't know about it and contradicts it
- Both sessions update MEMORY.md and the two versions conflict
- Work gets duplicated (both write the same utility function)

---

## The Solution: Divide by Domain, Own Your Files

### Rule 1: Each agent owns a domain

Before starting parallel sessions, assign clear file domains. Each session only writes to its domain.

```markdown
## Agent Domains — [DATE]

| Agent | Domain | Files it owns | Files it reads (not writes) |
|---|---|---|---|
| Agent A (Backend) | API + DB | src/api/**, src/db/**, supabase/** | src/types/**, memory/MEMORY.md |
| Agent B (Frontend) | UI + Components | src/components/**, src/pages/**, src/styles/** | src/types/**, src/api/types.ts |
| Agent C (Tests) | Test suite | src/**/*.test.ts, tests/** | Everything (read only) |
```

Add this table to MEMORY.md when a multi-agent session begins. Each session reads it and stays in its lane.

### Rule 2: Shared files are read-only for agents; human merges

Some files are read by all agents but written by none:
- `memory/MEMORY.md` — agents read this, but only the human updates it (or one designated "coordinator" agent)
- `src/types/index.ts` — shared type definitions; agents propose changes, human applies
- `CLAUDE.md` — rules file, only human edits

### Rule 3: Use a session coordination file

Create `memory/agent-log.md` for the duration of a multi-agent session:

```markdown
# Agent Log — [DATE]

## Agent A (Backend) — Last active: [TIME]
- Working on: User authentication endpoints
- Completed: POST /auth/login, POST /auth/register
- Decisions made: Using httpOnly cookies (not localStorage) for token storage
- Needs from Frontend: Confirm token storage approach before building refresh logic
- Blocking: Nothing

## Agent B (Frontend) — Last active: [TIME]
- Working on: Login page component
- Completed: LoginForm component with validation
- Decisions made: Using React Hook Form for form handling
- Needs from Backend: Token storage approach (cookie vs. header)
- Blocking: Waiting on Backend decision above

## Shared Decisions
- [TIME] Token storage: httpOnly cookies — agreed by both agents
```

Each agent updates only its own section. Human reviews conflicts.

### Rule 4: Commit before switching

Before starting a second agent session on the same project, commit all changes from the first. This gives you a clean restore point and prevents merge conflicts from accumulating.

```bash
git add -A && git commit -m "wip: backend auth endpoints — agent A session"
```

### Rule 5: Type definitions are a contract

If agents share types (TypeScript, Python dataclasses, DB schemas), treat the shared type file as a contract. Agent A proposes a type change in a comment; Agent B reviews before it's applied. Never let two agents modify the same type file simultaneously.

---

## Practical Setup

**In CLAUDE.md**, add a section:
```markdown
## Multi-Agent Mode (when active)
- This agent owns: [DOMAIN]
- Read-only files: memory/MEMORY.md, src/types/index.ts
- Coordination log: memory/agent-log.md
- When finished: update agent-log.md with completed work and decisions
```

**In MEMORY.md**, add when starting:
```markdown
## Active Agents (Multi-Agent Session)
- Agent A (Backend): owns src/api/, src/db/
- Agent B (Frontend): owns src/components/, src/pages/
- Coordination: memory/agent-log.md
- Human reviews before merging shared file changes
```

---

## When Not to Use Multiple Agents

Multi-agent coordination adds overhead. Use a single session unless:
- Work is clearly separable (backend vs. frontend with a defined API contract)
- Each domain is large enough to warrant its own session
- You're comfortable monitoring both sessions and reviewing a merge

For most projects, sequential single-agent sessions (one session, one context) are simpler and less error-prone.

---

## Success Metric

- No two agents modify the same file simultaneously
- Decisions made by one agent are recorded in agent-log.md and visible to others
- Merge conflicts are rare; when they happen, human resolves them
- Multi-agent sessions end with a commit and a clean MEMORY.md update

---

## See Also

- [Session Continuity](session-continuity.md) — single-agent context across sessions
- [File Safety](file-safety.md) — archive over delete, read before write
- [Working Scope](working-scope.md) — staying in assigned domain
