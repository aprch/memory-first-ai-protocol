# Patterns — Reference Documentation

> **These files are for HUMANS to understand the framework.**
> **AI should NOT read these every session.**

The patterns in this folder explain WHY the Memory-First AI Protocol works. The rules are already baked into `templates/CLAUDE.md.template`.

## When AI Should Read These

- When troubleshooting why something isn't working
- When you ask AI to explain a pattern
- When adapting the framework to a new use case

## When AI Should NOT Read These

- Every session (wastes tokens)
- Before starting work (rules are in CLAUDE.md)
- When working normally (context is in MEMORY.md)

---

## Which Patterns Do I Need?

### Starting a new project
1. [Session Continuity](session-continuity.md) — set up the two-file system
2. [Token Efficiency](token-efficiency.md) — keep memory lean from the start
3. [File Safety](file-safety.md) — archive over delete
4. [Secrets and Environment Variables](secrets-and-env.md) — set up `.gitignore` before first commit
5. [Working Scope](working-scope.md) — define boundaries

### Something went wrong
- AI made an error → [Error Recovery](error-recovery.md)
- AI is repeating a corrected mistake → [Learning Loops](learning-loops.md)
- AI seems confused or making things up → [Uncertainty Handling](uncertainty-handling.md)
- AI deleted or changed the wrong file → [File Safety](file-safety.md)

### Working in a team
- [Multi-Agent Coordination](multi-agent-coordination.md) — parallel AI sessions without conflicts
- See also: [examples/multi-dev-saas/](../examples/multi-dev-saas/) for a complete team setup

### Building a web project
- [Site File Architecture](site-file-architecture.md) — standard folder structures (React, WordPress)
- [Logo-First Site Development](logo-first-site-development.md) — create logo package before pages
- [SVG 3D Icon Pipeline](svg-3d-icon-pipeline.md) — logo creation and batch export
- [Deployment Workflows](deployment-workflows.md) — document deployment details in memory

### Managing a large project
- [Context Management](context-management.md) — index over dump, topic files for depth
- [Asset Organization](asset-organization.md) — organize outputs by batch with context files
- [Internal Tools](internal-tools.md) — tools folder with index
- [Cross-Project Tool Reuse](cross-project-tool-reuse.md) — audit existing tools before building new
- [Dependency Decisions](dependency-decisions.md) — track package choices so AI doesn't re-litigate them

### Working on a design/marketing project
- [Design Reference](design-reference.md) — analyze reference sites before implementing
- [Swipe File](swipe-file.md) — proven copy structures you reuse and improve
- [Prompt Engineering](prompt-engineering.md) — store reusable prompts as project assets

### WordPress specific
- [WP Block Validation](wp-block-validation.md) — avoid "Attempt Recovery" errors in FSE themes

---

## Full Pattern Index

| Pattern | What It Solves |
|---|---|
| [Asset Organization](asset-organization.md) | Scattered outputs, no context for old exports |
| [Context Management](context-management.md) | AI losing focus, bloated MEMORY.md |
| [Cross-Project Tool Reuse](cross-project-tool-reuse.md) | Duplicating tools across projects |
| [Dependency Decisions](dependency-decisions.md) | AI suggesting libraries you already evaluated |
| [Deployment Workflows](deployment-workflows.md) | Broken deploys, lost server config between sessions |
| [Design Reference](design-reference.md) | Random UI exploration, inconsistent design output |
| [Error Recovery](error-recovery.md) | When AI breaks something — rollback and prevention |
| [File Safety](file-safety.md) | Accidental deletions, overwritten work |
| [Internal Tools](internal-tools.md) | Lost scripts, duplicated tools, undocumented setup |
| [Learning Loops](learning-loops.md) | AI repeating the same corrected mistakes |
| [Logo-First Site Development](logo-first-site-development.md) | Building pages without logo assets ready |
| [Multi-Agent Coordination](multi-agent-coordination.md) | Parallel AI sessions conflicting on the same project |
| [Prompt Engineering](prompt-engineering.md) | Rewriting complex prompts from scratch every session |
| [Secrets and Environment Variables](secrets-and-env.md) | API keys leaking into memory files or git |
| [Session Continuity](session-continuity.md) | Starting from zero every chat session |
| [Site File Architecture](site-file-architecture.md) | Inconsistent project folder structures |
| [SVG 3D Icon Pipeline](svg-3d-icon-pipeline.md) | Logo/icon iteration sprawl, manual export to all sizes |
| [Swipe File](swipe-file.md) | Writing copy structure from scratch every time |
| [Token Efficiency](token-efficiency.md) | Wasted context on re-reading files, verbose memory |
| [Uncertainty Handling](uncertainty-handling.md) | AI hallucinating, stating things without verifying |
| [Working Scope](working-scope.md) | AI accessing files outside the project folder |
| [WP Block Validation](wp-block-validation.md) | WordPress block editor "Attempt Recovery" errors |

---

## The Core Insight

Most AI collaboration fails because people treat AI as stateless. They explain the same things every session. AI makes the same mistakes. Context is lost.

This framework makes AI stateful through files:
- **CLAUDE.md** = Persistent rules (you write once)
- **MEMORY.md** = Persistent context (AI maintains)
- **Corrections** = Learning (mistakes become rules)

The patterns explain how to make this work efficiently.
