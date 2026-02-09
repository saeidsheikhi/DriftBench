# SDR Benchmark Data Card (v1.0)

## Motivation

Schema evolution breaks production analytics and data applications. SDR measures how well automated agents (including LLM-based systems) can **repair SQL queries** under realistic schema drift.

## What is a single instance?

An SDR instance includes:
- Original schema (tables + column names/types)
- Drifted schema (after table/column renames)
- Drift operations + canonical migration plan
- A failing SQL query written for the original schema
- The real DB error produced when executing it on the drifted schema
- A ground-truth repaired SQL query produced via AST rewriting

## Composition

- Format: JSON Lines (`suite.jsonl`), 1 instance per line
- Task types: column rename, table rename, mixed; simple/aggregation/join/subquery queries
- Domains: synthetic e-commerce schema (customers, orders, products, order_items)

## Generation process

Instances are procedurally generated from a base schema. Drift operations are applied deterministically per seed, and queries are generated **conditioned** on drift so that the query is guaranteed to be affected.

Data used during evaluation is synthetic and generated deterministically from the seed, then migrated using the canonical migration plan.

## Intended use

- Benchmarking query-repair systems
- Studying robustness to schema evolution
- Ablations: error-only vs. error+schema-drift hint prompts

## Not intended for

- Measuring real-world user behavior
- Any setting requiring real personal data

## Ethics & privacy

All data is synthetic. No personal data is collected or distributed. Risks are mainly **misuse in automation contexts** (e.g., deploying unverified repairs). We recommend human review and query auditing for production deployment.

## Bias & limitations

- Single domain schema template (e-commerce). Results may not generalize to other schemas.
- Drift operations currently focus on renames; other drift types (split/merge/type changes/constraints) are future work.
- SQL dialect: PostgreSQL (SQLite fallback for development).

## Distribution

- Provide `suite.jsonl` via a public, no-request link (e.g., Zenodo DOI or Hugging Face dataset repository).
- Provide code via GitHub under an OSI license.

## Maintenance

- Versioned releases (e.g., v1.0, v1.0+)
- Include a manifest with SHA256 hashes for integrity.
