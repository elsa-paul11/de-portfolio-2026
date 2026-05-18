# ADR-001: Partition Strategy for Orders Bronze Layer

**Date:** 2026-05-15
**Status:** Accepted
**Author:** Elsa

## Context
Orders table ingested daily from MySQL into S3 as Parquet.
Analytics team filters most queries by date range and status.
Athena charges per TB scanned — partition strategy 
directly affects monthly cost.

## Decision
Partition by event_year + event_month.

## Alternatives Rejected

**Status partitioning:**
Only 4 unique values. Each partition = ~12.5TB.
Worse performance than no partitioning at all.

**event_day partitioning:**
At current volume, files would be ~5MB each.
Small files problem — S3 LIST API overhead outweighs benefit.
Revisit when daily volume exceeds 50GB/day.

**order_id / customer_id partitioning:**
Too high cardinality. Millions of tiny folders. Unusable.

## Consequences
✅ Queries filtering by year/month skip irrelevant partitions
✅ File sizes stay healthy at 128MB+ per partition
❌ Status filter still scans full month partition
⏳ Mitigated in Module 4 by Z-ordering on status column
