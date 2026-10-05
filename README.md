# aw-dbt

dbt project for the AdventureWorks analytics platform on Snowflake, run on dbt Cloud.

## Layout

```
models/
  staging/<source>/   1:1 with raw tables; rename + cast only
  intermediate/       reusable business logic
  marts/<domain>/     dim_* / fct_* consumed by BI
snapshots/            SCD2 history
macros/  seeds/  tests/  analyses/
docs/                 conventions and ADRs
```

## Environments

| Env | Snowflake DB | Who runs it |
|---|---|---|
| Development | `ADVENTUREWORKS_DEV.DBT_<USER>` | dbt Cloud CLI / IDE |
| CI | `ADVENTUREWORKS_CI.DBT_CLOUD_PR_*` | dbt Cloud Slim CI on PR |
| Production | `ADVENTUREWORKS_PROD` | dbt Cloud job on merge + schedule |

## Contributing

1. Branch from `main`: `feature/<ticket>-<desc>`
2. Develop with the dbt Cloud CLI: `dbt build --select +my_model`
3. `pre-commit run --all-files` before pushing
4. Open a PR; CI must pass; squash merge

See `docs/CONVENTIONS.md` and `docs/adr/`.
