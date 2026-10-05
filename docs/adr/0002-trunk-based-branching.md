# ADR-0002: Trunk-based development with squash merges

- **Status:** Accepted
- **Date:** 2026-10-05
- **Deciders:** Peng

## Context

In dbt and Terraform there is no packaged release artefact: production is
whatever code is on the deployed branch. dbt Cloud Slim CI compares a PR to
the production manifest; that comparison is only meaningful if the default
branch equals production.

## Decision

`main` is the only long-lived branch and always equals production. All work is
done on short-lived branches (`feature/`, `fix/`, `chore/`, `docs/`) and merged
through a PR using squash merge. Commit and PR titles follow Conventional
Commits. Hotfixes use the same path with expedited review.

## Alternatives considered

- **GitFlow (`develop` + `release/*` + `hotfix/*`)** — rejected: adds a second
  integration branch that drifts from production, breaks the Slim CI premise,
  and adds merge ceremony with no benefit for a continuously deployed platform.
- **Merge commits instead of squash** — rejected: noisy history, harder reverts.

## Consequences

- Branches must stay small; large changes are split into sequential PRs.
- Reverting a feature is `git revert` of one commit.
- Feature flags / `enabled: false` in dbt may be needed for partially built work.
