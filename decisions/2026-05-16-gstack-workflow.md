---
id: "decision-20260516-002"
type: decision
status: implemented
date: 2026-05-16
agent: hermes
tags: [gstack, workflow, methodology, office-hours, build-loop]
supersedes: []
---

# Decision: GStack Workflow for Project Audits and Execution

## Context
Managing multiple projects (hermes-agent, hermes-workspace, agentic-etl-stack, hermes-infra) required a repeatable audit → plan → execute cycle. Without structure, each project review was ad-hoc and inconsistent.

## Options Considered
1. **GStack Workflow** (chosen): Structured 6-skill pipeline — office-hours → plan-ceo-review → plan-eng-review → plan-design-review → plan-devex-review → build-loop
2. **Ad-hoc per project**: No structure, just start coding — leads to scope creep and inconsistency
3. **Single monolithic plan**: One big planning document — rigid and doesn't compose across projects

## Decision
Adopt the GStack workflow as the standard methodology for all project interventions:

1. **office-hours** — Audit mode for existing projects (7-dimension diagnostic), Startup/Builder mode for new ideas
2. **plan-ceo-review** — Scope determination: HOLD SCOPE / SCOPE EXPANSION / SCOPE REDUCTION / CANCEL
3. **plan-eng-review** — Architecture lock: data flow, API design, component tree
4. **plan-design-review** — UI/UX verification (10 dimensions)
5. **plan-devex-review** — Developer experience audit
6. **build-loop (RalphLoop)** — Autonomous cron-based execution with delegate_task subagents

Key principle: Each skill must be invoked independently — the user chooses to proceed or stop at each gate. The workflow is a guide, not a straitjacket.

## Consequences
- **Consistency**: Every project gets the same rigorous treatment
- **Traceability**: Each step produces a written artifact (diagnostic, plan, decision log)
- **Speed**: Projects that pass CEO review quickly can skip design/devex if not needed
- **Energy**: 14KB across decision documents and skills

## References
- Skills: `office-hours`, `plan-ceo-review`, `plan-eng-review`, `plan-design-review`, `plan-devex-review`, `build-loop`
- Decisions: agentic-etl-stack cleanup, workspace APIs architecture
- Agent memory: session-gstack-may-15
