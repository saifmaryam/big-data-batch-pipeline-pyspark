# 🗄️ Big Data Batch Processing Pipeline (PySpark)

> A distributed batch-processing pipeline built with Apache Spark to clean, transform, and analyze half a million e-commerce transaction records.

## Why This Project?

Most portfolio projects that claim to use "Spark" only run `df.show()` on a tiny CSV. This one is built specifically to exercise the parts of PySpark that actually matter at scale: **deduplication, null handling, groupBy aggregations, window functions, and columnar storage** — the operations a Data Engineer runs daily against production-sized datasets.

## What It Processes

A synthetic e-commerce dataset of **501,000 transactions** — deliberately generated with realistic messiness (missing prices, duplicate transaction IDs) to mirror what raw data actually looks like before it reaches a data warehouse.

## Pipeline Stages

```
Generate (501,000 rows) → Load into Spark → Clean (→ 495,000 rows)
        → Aggregate (revenue by category, monthly trend)
        → Rank (top customers via window function)
        → Write to Parquet
```

| Stage | What Happens |
|-------|-------------|
| **Generate** | Simulates 501K transactions across 7 categories, 4 regions, and 4 payment methods, with intentional nulls and duplicates injected |
| **Load** | Converts to a distributed Spark DataFrame and validates schema |
| **Clean** | Drops null prices, deduplicates by transaction ID, filters invalid quantities/prices — 501,000 → 495,000 valid records |
| **Aggregate** | Computes total revenue and order count per category, and revenue trend across all 12 months |
| **Rank** | Uses a Spark **Window function** to rank customers by total spend — the same pattern used for leaderboards, top-N reports, and running totals in production systems |
| **Store** | Writes the cleaned dataset to **Parquet**, then re-reads it to verify record integrity |

## Sample Results

- **Cleaning:** 501,000 → 495,000 records after removing nulls, duplicates, and invalid rows
- **Top category by revenue:** Home & Kitchen (~89.8M in total revenue, ~70.8K orders)
- **Top spender:** A single customer with over 253K in lifetime spend, identified via `RANK() OVER (ORDER BY total_spent DESC)`
- **Output verified:** 495,000 records confirmed present after writing to and reading back from Parquet

## Tech Stack

`Python` · `Apache Spark (PySpark)` · `Pandas` · `NumPy` · `Parquet`

## Key Concepts Demonstrated

- Distributed DataFrame operations (`groupBy`, `agg`, `filter`, `dropDuplicates`)
- Window functions for ranking (`Window.orderBy`, `rank()`)
- Data quality handling (null removal, deduplication, invalid-row filtering)
- Columnar storage with Parquet, including a read-back verification step

## Run It Yourself

1. Open `Big_Data_Batch_Pipeline_PySpark.ipynb` in Google Colab
2. Runtime → Run all
3. No external dataset or setup required — the data is generated inline

## Author

**Maryam Saif** — [GitHub](https://github.com/saifmaryam)
