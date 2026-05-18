# Module 01 — Physical Storage

[![Open In Colab](https://github.com/elsa-paul11/de-portfolio-2026/blob/main/module-01-storage/notebooks/m1_d1_s3_parquet_pipeline.ipynb)]

## What I Built
Partitioned Parquet ingestion pipeline on AWS S3 using PySpark.
10,000 orders ingested, partitioned by event_year + event_month,
stored as Snappy-compressed Parquet.

## Stack
- PySpark 3.5
- AWS S3 (ap-south-1)
- Apache Parquet + Snappy compression
- boto3

## Key Decisions
- Parquet over CSV: columnar format, 10x compression, predicate pushdown
- Partitioned by event_year + event_month: reduces Athena scan by 50%
- See ADR-001 for full reasoning

## Architecture Decision Records
- [ADR-001: Partition Strategy](adr/ADR-001-partition-strategy.md)

## Files
- [Pipeline Notebook](notebooks/m1_d1_s3_parquet_pipeline.ipynb)
  
