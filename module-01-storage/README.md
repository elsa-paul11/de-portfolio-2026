# Module 01 — Physical Storage

## What I built
Partitioned Parquet ingestion pipeline on AWS S3 using PySpark.

## Stack
- PySpark 3.5
- AWS S3 (ap-south-1)
- Apache Parquet + Snappy compression
- boto3

## Key Decisions
- Parquet over CSV: columnar format, 10x compression
- Partitioned by event_year + event_month
- See ADR-001 for full reasoning

## Files
- notebooks/m1_d1_s3_parquet_pipeline.ipynb
- adr/ADR-001-partition-strategy.md
  
