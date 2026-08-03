# Module 02 — Spark Internals

## What I Built
PySpark jobs demonstrating lazy evaluation, shuffle mechanics,
join strategies, and data skew with salting fix.
Measured real timing differences between shuffle and non-shuffle
operations, and between Sort Merge Join and Broadcast Join.

## Key Concepts
- Lazy evaluation — transformations vs actions
- Execution plans — reading explain() output
- Shuffles — the most expensive Spark operation
- Sort Merge Join — 2 shuffles + 2 sorts
- Broadcast Join — 0 shuffles, copies small table to all workers
- Data skew — one partition doing 70% of work
- Salting — artificially spreading skewed keys across partitions

## Architecture Decision Records
- [ADR-002: Join Strategy](adr/ADR-002-join-strategy.md)
- [ADR-003: Salting Strategy](adr/ADR-003-salting-strategy.md)

## Files
- [Spark Internals Notebook](notebooks/m2_d2_spark_internals.ipynb)
- [Skew and Salting Notebook](notebooks/m2_d3_skew_and_salting.ipynb)
