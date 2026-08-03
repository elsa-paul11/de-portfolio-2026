Decision: Use SCD Type 2 for customer dimension
Reason:   Business requires point-in-time historical accuracy
          for revenue attribution by customer tier and city
Rejected: SCD Type 1 — overwrites history, breaks historical reports
          SCD Type 3 — only one level of history, insufficient
Implementation: Delta Lake MERGE (atomic close + insert)
Consequence: Table grows with every change. Monitor size quarterly.
