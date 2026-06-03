# ADR-002: Join Strategy for Orders + Customers Pipeline

**Date:** 2026-06-01
**Status:** Accepted
**Author:** Elsa

## Context
Orders pipeline joins two tables on customer_id:
- orders table: large, grows daily (currently 10,000 rows, 
  production target 500GB+)
- customers table: small, changes slowly (currently 5,000 rows,
  production target ~10MB)

Join strategy directly affects shuffle cost and job duration.
Wrong join type at 500GB = 45 minute job instead of 5 minute job.

## Decision
Use Broadcast Join for orders + customers join.
Explicitly wrap small table with broadcast() function.
Do not rely on Spark's auto-broadcast threshold alone.

## Why Broadcast Join

Sort Merge Join cost:
→ 2 shuffles (orders + customers both move across network)
→ 2 sorts (both tables sorted by join key)
→ At 500GB orders: 45+ minutes

Broadcast Join cost:
→ 0 shuffles (orders table never moves)
→ 0 sorts
→ customers table copied once to every worker as hash map
→ At 500GB orders: ~3-5 minutes

## Why Not Sort Merge Join

customers table is small and changes slowly.
Shuffling it on every job run wastes network and time.
No benefit to Sort Merge when one table fits in memory.

## Alternatives Rejected

Auto-broadcast only (no explicit broadcast() call):
Default threshold = 10MB.
customers table may grow beyond 10MB over time.
Auto-broadcast would silently stop working.
Explicit broadcast() makes the decision visible and intentional.

Bucketing:
Pre-partitioning both tables by customer_id eliminates shuffle.
Valid option at very large scale (both tables 100GB+).
Overkill for current data size.
Deferred until both tables exceed 10GB.

## Broadcast Size Limit

Default broadcast threshold: 10MB
(spark.sql.autoBroadcastJoinThreshold)

customers table current size: ~1MB
customers table projected size: ~50MB in 2 years

Action required when customers exceeds 10MB:
→ Increase threshold explicitly:
  spark.conf.set("spark.sql.autoBroadcastJoinThreshold", "100MB")
→ Or switch to bucketing strategy
→ Review annually

## Consequences
✅ Eliminates 2 shuffles and 2 sorts per job run
✅ orders table (large) never moves across network
✅ Job runtime: 45 minutes → ~5 minutes at 500GB
❌ customers table must fit in each worker's RAM
❌ If customers grows beyond worker RAM: OutOfMemoryError
⏳ Monitor customers table size monthly
⏳ Switch to bucketing when customers exceeds 500MB
