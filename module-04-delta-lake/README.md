# Module 04 — Delta Lake

## What I Built
SCD Type 2 customer dimension pipeline using Delta Lake MERGE.
Point-in-time historical queries verified.
Z-ordering implemented for query performance.

## Key Concepts
- ACID transactions on S3 via Delta transaction log
- Time travel — query any historical version
- SCD Type 2 — full history with valid_from/valid_to/is_current
- Delta MERGE — atomic close + insert in one operation
- OPTIMIZE + ZORDER BY for file skipping

## Architecture Decision Records
- [ADR-004: SCD Strategy](adr/ADR-004-scd-strategy.md)

## Files
- [Delta Lake Notebook](notebooks/m4_delta_lake.ipynb)
