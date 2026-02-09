# DriftBench v1.0

This repository contains the **DriftBench** benchmark: a procedurally generated set of SQL repair tasks where a query written for an **original schema** fails after **schema drift** (table/column renames), and systems must rewrite the query to execute on the **drifted schema** while preserving semantics.

## What’s included

- `suite.jsonl` — benchmark task instances (schemas, drift operations, failing queries, ground-truth repairs, metadata).
- `results.csv` — example run results (execution success, semantic equivalence, oracle score, edit distance, latency).
- `responses_main.jsonl` — prompts and raw model outputs for the example run (optional; can be large).
- `fig*.png`, `*.tex` — paper-ready figures/tables produced from the run.

## Quickstart (reproduce an evaluation run)

1. **Install dependencies**
   - Python 3.10+
   - PostgreSQL 15+ (recommended) or SQLite fallback
   - Python packages: `sqlglot`, `psycopg2-binary`, `pandas`, `numpy`, `matplotlib`

2. **Run the notebook / script**
   - Open `Schema_Drift_Repair_PAPER_READY-1000_FIXED.ipynb` and run all cells.

3. **Outputs**
   - The run writes outputs under `artifacts/` (results CSV, JSONL logs, figures, LaTeX tables).

## Data format (suite.jsonl)

Each line is a JSON object describing one benchmark instance. Key fields:

- `instance_id`: unique ID
- `seed`: generation seed
- `original_schema`: tables/columns/types for schema version t
- `drifted_schema`: schema after drift
- `drift_operations`: list of drift ops (canonical references)
- `migration_plan`: canonical mapping for data migration
- `original_query`: query that fails on drifted schema
- `ground_truth_repair`: AST-generated repair (oracle)
- `difficulty`, `drift_category`, `query_complexity`

See `DATACARD.md` for full documentation and intended use.

## Security note

If you evaluate against a hosted LLM API, **do not commit API keys/tokens** to public repositories. Load them from environment variables or local config files.

## License

- Code: Apache-2.0 (recommended)
- Benchmark data (`suite.jsonl`): CC-BY-4.0 (recommended)

## Citation

A `CITATION.cff` template is provided. Update author/title/year before release.
