# Database and Query Languages

Use this reference for SQL, migration SQL, stored procedures, views, triggers, and database-query files. Verify the database engine and migration framework before relying on dialect-specific syntax.

## Comment Syntax

Use `--` for line comments and `/* ... */` for larger explanations only when supported by the target engine and surrounding tooling. Follow repository convention. Never add dialect-specific constructs merely for documentation.

## What to Document

Prioritize:

- business meaning of non-obvious queries;
- join rationale and expected cardinality;
- CTE purpose and phase boundaries;
- unusual filters, sentinel values, and intentional `NULL` handling;
- constraints and data invariants;
- transaction boundaries, isolation, locks, and ordering;
- deduplication, reconciliation, and conflict strategy;
- destructive operations and why their scope is safe;
- backfill assumptions, batching, resumability, and performance impact;
- data transformations, ownership, retention, and lifecycle;
- index or execution-plan-sensitive choices;
- rollback limitations and operational prerequisites.

Comment logical phases and assumptions rather than every clause.

```sql
-- --- Step 1: Select the latest successful payment for each invoice ---
WITH latest_payment AS (
    ...
),
-- Reversed payments are removed here so downstream totals never subtract them twice.
active_payments AS (
    ...
)
SELECT ...;
```

## Transactions and Concurrency

When verified, explain why a transaction starts and ends where it does, what must be atomic, expected lock scope, retry behavior, isolation assumptions, and races prevented by constraints or locking. Do not claim atomicity solely because statements appear next to one another.

## Destructive and Data-Changing Queries

For `DELETE`, `UPDATE`, DDL, truncation, backfills, and repair queries, document target scope, safety guards, invariants, expected row population, rerun behavior, failure recovery, and validation steps when non-obvious. Do not add examples containing production data.

## Migrations

Be conservative. Do not edit already-applied or checksum-sensitive migrations without explicit project permission. For new or safe-to-edit migrations, document intent not obvious from filenames and statements:

- why the schema/data change is needed;
- lock and deployment implications;
- assumptions about existing rows;
- backfill sequencing;
- compatibility windows between application versions;
- irreversible steps and rollback limits.

Do not rewrite, reorder, or “clean up” migration code just to improve comments.

## Stored Programs, Views, and Triggers

Use the database's supported documentation/comment mechanisms. Explain external contract, invocation assumptions, side effects, recursion or trigger ordering, security context, and failure behavior. Prefer database-native metadata descriptions where established.

## Query Builders and Embedded SQL

Load the host-language reference as well. Put API-contract documentation in the host language and SQL-specific rationale beside the query construction. Avoid duplicating the same explanation in both places.

## Review

Use the project's SQL parser, migration status/checksum tool, formatter, tests, and explain-plan workflow when available. Verify the dialect, comment placement, cardinality claims, transaction semantics, and migration mutability before finalizing.
