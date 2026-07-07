Decision: Use mode=overwrite with dynamic partition overwrite
          for all pipeline writes, never mode=append
Reason:   Airflow retries failed tasks automatically.
          Append mode creates duplicates on retry.
          Overwrite mode is safe to run N times — same result.
Consequence: Slightly higher write cost vs append.
             Accepted — data correctness is non-negotiable.
