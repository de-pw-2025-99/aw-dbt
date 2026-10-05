# ADR-0003: dbt Cloud as transformation and scheduling platform

- **Status:** Accepted
- **Date:** 2026-10-05
- **Deciders:** Peng

## Context

The team already licenses dbt Cloud. The transformation layer is dbt; it needs
a scheduler, CI execution environment, documentation hosting and alerting.

## Decision

Use dbt Cloud for: Development / CI / Production environments, Slim CI jobs on
PRs, the production job on merge and on schedule, docs hosting and alerts.
Developers use VS Code with the dbt Cloud CLI rather than the browser IDE.
Code linting runs in GitHub Actions, not in dbt Cloud.

## Alternatives considered

- **dbt Core + Airflow for everything** — rejected for now: more to operate;
  revisit (ADR) if orchestration needs outgrow dbt Cloud (see Phase 9).
- **dbt Core + GitHub Actions as scheduler** — rejected: no deferral/state
  management, no UI, cron in CI is fragile.

## Consequences

- Credentials for Snowflake live in dbt Cloud, not in the repo or CI.
- GitHub Actions is limited to static checks (lint, parse); anything needing a
  warehouse runs in dbt Cloud.
- Terraform may later manage dbt Cloud itself via its provider.
