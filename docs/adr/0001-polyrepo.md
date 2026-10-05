# ADR-0001: Separate repositories for infrastructure and dbt (polyrepo)

- **Status:** Accepted
- **Date:** 2026-10-05
- **Deciders:** Peng

## Context

The platform has two independently changing concerns: Snowflake infrastructure
(databases, warehouses, roles, grants — managed with Terraform) and the dbt
transformation project. They have different owners in a typical team, different
risk profiles (a bad `terraform apply` can drop a database; a bad dbt merge
produces wrong data but is recoverable), and different CI pipelines. dbt Cloud
expects a repository whose root (or a declared subdirectory) is a dbt project.

## Decision

Use one repository per concern: `aw-snowflake-infra` (Terraform) and `aw-dbt`
(dbt). Later concerns (orchestration) get their own repository. A local parent
folder holds all clones but is not itself a Git repository.

## Alternatives considered

- **Monorepo with `terraform/` and `dbt/` subfolders** — rejected: every
  workflow needs path filters; permission to merge dbt implies permission to
  apply infra; dbt Cloud CI would trigger on infra-only changes; nested-repo
  mistakes are easy.
- **Single repo, dbt only, infra by hand** — rejected: infra would not be
  reproducible or reviewable.

## Consequences

- Changes spanning infra + dbt need two PRs, merged infra first.
- Conventions (branching, PR template, ADR format) are duplicated across repos
  and must be kept aligned by hand.
- Each repo gets focused CI and its own CODEOWNERS.
