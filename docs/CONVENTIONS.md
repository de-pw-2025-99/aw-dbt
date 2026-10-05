# Conventions

These apply across all AdventureWorks platform repositories.

## Git

| Thing | Rule | Example |
|---|---|---|
| Default branch | `main` — always deployable, equals production | |
| Feature branches | `feature/<ticket>-<desc>`, `fix/...`, `chore/...`, `docs/...` | `feature/AW-12-dim-customer` |
| Branch lifetime | Days, not weeks. Merge or delete. | |
| Merge method | Squash merge only (linear history) | |
| Commit / PR title | Conventional Commits | `feat: add dim_customer` |
| Direct push to `main` | Never | |

Conventional Commit types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `ci`, `perf`.

## Snowflake objects (UPPER_SNAKE_CASE)

| Object | Pattern | Examples |
|---|---|---|
| Database | `ADVENTUREWORKS_<ENV>` | `ADVENTUREWORKS_DEV`, `_CI`, `_PROD`; `ADVENTUREWORKS_RAW` is the landing zone |
| Schema | dbt layer name | `STAGING`, `INTERMEDIATE`, `MARTS`, `SNAPSHOTS` |
| Dev schemas | `DBT_<USERNAME>` in `ADVENTUREWORKS_DEV` | `DBT_PENG` |
| CI schemas | `DBT_CLOUD_PR_<job>_<pr>` (dbt Cloud managed) | |
| Warehouse | `WH_<PURPOSE>_<ENV>` | `WH_TRANSFORM_DEV`, `WH_TRANSFORM_PROD`, `WH_REPORTING` |
| Functional role | verb-based, never personal | `LOADER`, `TRANSFORMER_DEV`, `TRANSFORMER_PROD`, `REPORTER` |
| Service user | `SVC_<SYSTEM>_<ENV>` | `SVC_DBT_PROD`, `SVC_DBT_CI`, `SVC_TERRAFORM` |

## dbt models (lower_snake_case)

| Layer | Pattern | Example | Materialisation |
|---|---|---|---|
| Staging | `stg_<source>__<entity>` | `stg_sales__salesorderheader` | view |
| Intermediate | `int_<entity>_<verb>` | `int_orders_joined_to_lines` | ephemeral / view |
| Marts | `dim_<entity>` / `fct_<event>` | `dim_customer`, `fct_sales_order` | table / incremental |
| Snapshot | `snap_<source>__<entity>` | `snap_sales__customer` | snapshot |

Rules:
- Staging = one model per source table, 1:1, renaming + casting only. No joins.
- Business logic lives in intermediate and marts, never in staging.
- Every mart has a declared primary key tested `unique` + `not_null`.
- Every model has a description. Every mart column has a description.

## Columns

| Kind | Rule | Example |
|---|---|---|
| Surrogate key | `<entity>_key` | `customer_key` |
| Natural / source key | `<entity>_id` | `customer_id` |
| Boolean | `is_` / `has_` prefix | `is_online_order` |
| Timestamp | `_at` suffix | `modified_at` |
| Date | `_date` suffix | `order_date` |
| Amount | `_amount`, currency implied AUD unless suffixed | `total_due_amount` |

## YAML

- One `_<source>__sources.yml` and `_<source>__models.yml` per staging folder.
- One `_<domain>__models.yml` per marts folder.
- Leading underscore sorts the YAML to the top of the folder.
