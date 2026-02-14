---
name: pg-gleam
description: Postgres performance optimization and best practices for a Gleam + Squirrel + POG + Cigogne stack. Use this skill when writing, reviewing, or optimizing Postgres queries, schema designs, or database configurations.
license: MIT
metadata:
  version: "2.0.0"
  date: February 2026
  abstract: Postgres best practices aligned with a Gleam backend stack (Squirrel for SQL codegen, POG for connections, Cigogne for migrations). Covers schema design, query performance, connection management, security, and patterns specific to our conventions (UUIDv7, bigint money, session-variable RLS, prefixed enums, soft deletes, audit fields).
model: sonnet
memory: project
color: green
---

# Postgres Best Practices (Gleam Stack)

Performance optimization and schema design guide for Postgres, aligned with our Gleam backend stack.

## Stack Context

| Layer       | Tool      | Role                                        |
| ----------- | --------- | ------------------------------------------- |
| Language    | Gleam     | Backend application code                    |
| SQL codegen | Squirrel  | Generates type-safe Gleam from `.sql` files |
| DB driver   | POG       | Connection pooling, binary protocol         |
| Migrations  | Cigogne   | Schema migrations                           |
| Extensions  | pg_uuidv7 | UUIDv7 primary key generation               |

## Our Conventions

- **Primary keys**: `uuid` via `uuid_generate_v7()` (pg_uuidv7 extension)
- **Timestamps**: `timestamp` (not `timestamptz`); timezone stored in `core.tenant.timezone`
- **Money**: `bigint` with scale factor 10,000 (maps to Gleam `Int`, exact arithmetic)
- **Enums**: `CREATE TYPE` with prefixed values (`os_pending`, `ps_paid`) for Gleam uniqueness
- **Constraints**: Named with `pk_`, `fk_`, `uq_`, `chk_`, `idx_` prefixes
- **Schemas**: `core`, `tenant`, `shared`, `extensions` separation
- **Tables**: Singular names (`order` not `orders`)
- **Soft deletes**: `deleted_at timestamp` + `deleted_by uuid` + partial indexes
- **Audit fields**: `created_at`, `updated_at`, `created_by`, `updated_by` via triggers
- **RLS**: Session variables via `core.current_tenant_id()` / `core.current_user_id()`

## Gleam Type Mapping (via Squirrel/POG)

| Postgres Type | Gleam Type             | Notes                      |
| ------------- | ---------------------- | -------------------------- |
| `uuid`        | `String`               | UUIDv7 for PKs             |
| `text`        | `String`               | Prefer over `varchar(n)`   |
| `boolean`     | `Bool`                 |                            |
| `integer`     | `Int`                  |                            |
| `bigint`      | `Int`                  | Use for money (exact)      |
| `numeric`     | `Float`                | **Lossy!** Avoid for money |
| `timestamp`   | `pog.Timestamp`        | No `timestamptz` support   |
| `date`        | `pog.Date`             |                            |
| `jsonb`       | `String` (raw JSON)    | Decode in Gleam            |
| `bytea`       | `BitArray`             |                            |
| `enum`        | Generated variant type | See `schema-enums.md`      |

## When to Apply

Reference these guidelines when:

- Writing SQL queries or designing schemas
- Implementing indexes or query optimization
- Reviewing database performance issues
- Configuring POG connection pooling
- Working with Row-Level Security (RLS)
- Creating Squirrel `.sql` query files
- Writing Cigogne migrations

## Rule Categories by Priority

| Priority | Category                 | Impact      | Prefix      |
| -------- | ------------------------ | ----------- | ----------- |
| 1        | Query Performance        | CRITICAL    | `query-`    |
| 2        | Connection Management    | CRITICAL    | `conn-`     |
| 3        | Security & RLS           | CRITICAL    | `security-` |
| 4        | Schema Design            | HIGH        | `schema-`   |
| 5        | Concurrency & Locking    | MEDIUM-HIGH | `lock-`     |
| 6        | Data Access Patterns     | MEDIUM      | `data-`     |
| 7        | Monitoring & Diagnostics | LOW-MEDIUM  | `monitor-`  |
| 8        | Advanced Features        | LOW         | `advanced-` |

## Token Efficiency

**Follow `references/token-efficiency.md` rules.** When reviewing or writing database code:

1. Grep for specific SQL patterns (e.g., `pog.transaction`, `set_config`, `FORCE ROW LEVEL`) instead of reading all migration files
2. Use `git diff --name-only` to scope reviews to changed `.sql` files only
3. Read targeted line ranges around Grep matches, not entire migrations

## How to Use

Read individual rule files in `references/` for detailed explanations and SQL examples. Each rule file contains:

- Brief explanation of why it matters
- Incorrect SQL example with explanation
- Correct SQL example with explanation
- Optional EXPLAIN output or metrics
- Gleam/POG-specific notes where applicable

## Available References

**Advanced Features** (`advanced-`):

- `references/advanced-deferred-constraints.md`
- `references/advanced-full-text-search.md`
- `references/advanced-generated-columns.md`
- `references/advanced-jsonb-indexing.md`

**Connection Management** (`conn-`):

- `references/conn-idle-timeout.md`
- `references/conn-limits.md`
- `references/conn-pooling.md`
- `references/conn-prepared-statements.md`

**Data Access Patterns** (`data-`):

- `references/data-batch-inserts.md`
- `references/data-n-plus-one.md`
- `references/data-pagination.md`
- `references/data-upsert.md`

**Concurrency & Locking** (`lock-`):

- `references/lock-advisory.md`
- `references/lock-deadlock-prevention.md`
- `references/lock-short-transactions.md`
- `references/lock-skip-locked.md`

**Monitoring & Diagnostics** (`monitor-`):

- `references/monitor-explain-analyze.md`
- `references/monitor-pg-stat-statements.md`
- `references/monitor-vacuum-analyze.md`

**Query Performance** (`query-`):

- `references/query-composite-indexes.md`
- `references/query-covering-indexes.md`
- `references/query-index-types.md`
- `references/query-missing-indexes.md`
- `references/query-partial-indexes.md`

**Schema Design** (`schema-`):

- `references/schema-audit-fields.md`
- `references/schema-data-types.md`
- `references/schema-enums.md`
- `references/schema-foreign-key-indexes.md`
- `references/schema-lowercase-identifiers.md`
- `references/schema-naming-conventions.md`
- `references/schema-partitioning.md`
- `references/schema-primary-keys.md`
- `references/schema-soft-deletes.md`

**Security & RLS** (`security-`):

- `references/security-privileges.md`
- `references/security-rls-basics.md`
- `references/security-rls-performance.md`

---

_36 reference files across 8 categories_

## References

- <https://www.postgresql.org/docs/current/>
- <https://wiki.postgresql.org/wiki/Performance_Optimization>
- <https://hexdocs.pm/pog/>
- <https://hexdocs.pm/squirrel/>
