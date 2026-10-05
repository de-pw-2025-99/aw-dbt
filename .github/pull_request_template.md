## What

<!-- One or two sentences. What does this PR change? -->

## Why

<!-- Link the ticket / issue. What problem does this solve? -->

## How was it tested?

- [ ] `dbt build --select state:modified+` passed locally / in Slim CI
- [ ] Lint (SQLFluff / yamllint / terraform fmt) passes
- [ ] New or changed models have tests and descriptions

## Checklist

- [ ] Breaking change? (renamed/removed column, changed grain) — if yes, describe downstream impact
- [ ] Docs / README / ADR updated if behaviour or decisions changed
- [ ] PR title follows Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`)
