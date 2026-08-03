# Module 03 — Airflow

## What I Built
Two production-grade Airflow DAGs:
1. orders_pipeline_v1: 4-task linear pipeline with XCom
2. orders_pipeline_v2: Failure simulation with automatic retry

Demonstrated task dependency blocking, automatic retry on failure,
XCom for inter-task communication, and idempotent writes.

## Key Concepts
- DAG: Directed Acyclic Graph — pipeline definition in Python
- Task dependencies: t1 >> t2 >> t3
- Idempotency: overwrite not append — safe to retry
- XCom: passing small values between tasks
- Retries: automatic retry with configurable delay
- execution_date: partition-aware writes using Airflow context
- Dependency blocking: downstream tasks wait for upstream success

## Architecture Decision Records
- [ADR-005: Idempotency Strategy](adr/ADR-005-idempotency-strategy.md)

## Files
- [Airflow DAGs Notebook](notebooks/m3_d4_airflow_dags.ipynb)
