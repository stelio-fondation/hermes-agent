---
id: "decision-20260515-001"
type: decision
status: implemented
date: 2026-05-15
agent: hermes
tags: [agentic-etl-stack, cleanup, architecture, gstack-workflow]
supersedes: []
---

# Decision: Simplify agentic-etl-stack to PostgreSQL + Metabase

## Context
Agentic ETL Stack was running 5 Docker containers (postgres, knime, superset, metabase, pgadmin) consuming 45% disk (86G/193G). GStack workflow (office-hours → plan-ceo-review → plan-eng-review) identified dead weight.

## Options Considered
1. **HOLD SCOPE** (chosen): Remove non-functional/redundant services, keep core
2. **SCOPE EXPANSION**: Rebuild KNIME with correct entrypoint, add more features
3. **SCOPE REDUCTION**: Strip everything except PostgreSQL

## Decision
Remove KNIME Executor (16GB dead weight, sleep infinity), Apache Superset (doublon BI), and pgAdmin (redondant avec psql CLI). Keep PostgreSQL 16 + Metabase.

## Consequences
- **Disk**: 86G → 60G (-26GB, 45% → 31%)
- **RAM**: ~10GB → ~3GB freed
- **Images**: 5 → 2 containers
- **Indexes**: 5 indexes added on stock_unite_legale (25.6M rows)
- **Maintenance**: Cron VACUUM ANALYZE chaque dimanche 3h

## References
- fact_store id: 18
- git commits: d074908, 80ce31c, c395e6b, b95d77b
- Links: agentic-etl-stack, hermes-agent (GStack skills)
