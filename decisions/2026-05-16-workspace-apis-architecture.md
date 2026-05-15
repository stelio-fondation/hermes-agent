---
id: "decision-20260516-001"
type: decision
status: implemented
date: 2026-05-16
agent: hermes
tags: [hermes-workspace, api, architecture, build-loop, decisions]
supersedes: []
---

# Decision: Workspace APIs for Build Loop and Decisions

## Context
Hermes Workspace dashboard needed to surface two new data sources — build loop run history (from agentic-etl-stack PostgreSQL) and architecture decisions (from markdown files in hermes-agent). Rather than embedding these queries in the client or adding a heavy backend, we created lightweight API routes.

## Options Considered
1. **Server-side API routes** (chosen): Thin server endpoints that proxy/fetch data and return JSON
2. **Client-side direct DB**: Bypass API, query PostgreSQL directly from the browser — inappropriate for security
3. **Embed in existing overview endpoint**: Would couple unrelated data into `/api/dashboard/overview` and increase its payload weight

## Decision
Create two independent API routes in hermes-workspace:
- `GET /api/build-loop` — queries `etl.build_loop_runs` table via `docker exec psql` with aggregated stats (runs, avg duration, success rate)
- `GET /api/decisions` — reads `/root/hermes-agent/decisions/*.md` files with YAML frontmatter parsing

Both routes are authenticated via the existing `isAuthenticated` middleware and cache-friendly (max-age=10s / stale-while-revalidate=30s).

## Consequences
- **Build loop API**: Returns typed `BuildLoopResponse` with `runs`, `latest`, `totals` — sufficient for a dashboard card without over-fetching
- **Decisions API**: Returns `Decision[]` with id, title, date, status, tags, summary — parsed from YAML frontmatter, no database dependency
- **Scalability**: Each route can evolve independently without touching the other
- **Security**: The build-loop route is scoped by docker exec + psql to the analytics database only. The decisions route only reads markdown, never writes.
- **Energy**: 2% disk space at 2.5KB each

## References
- Files: `/root/hermes-workspace/src/routes/api/build-loop.ts`, `/root/hermes-workspace/src/routes/api/decisions.ts`
- Components: `build-loop-card.tsx`, `decisions-card.tsx`
