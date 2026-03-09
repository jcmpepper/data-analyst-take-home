# Submission - [Your Name]

Replace `[Your Name]` with your actual name.

---

## Part 1: Data Quality & SQL

### Q1: Data Quality Issues

| File | Field(s) | Issue | How to Handle |
|------|----------|-------|---------------|
| | | | |

---

### Q2: Revenue by Category (Completed Orders)

```sql
-- Your query here
```

**Results:**

| category | total_revenue | pct_of_total |
|----------|---------------|--------------|
| | | |

---

### Q3: Top 3 Customers by Region

```sql
-- Your query here
```

---

### Q4: Order Frequency Increase

```sql
-- Your query here
```

---

### Q5: Average Order Value by Customer Tier (JSON extraction)

```sql
-- Your query here
```

---

## Part 2: dbt & Configuration

### Q6: Materialization Strategy for 50M+ Rows

[Your answer here]

---

### Q7: Debug the YAML Config

**Errors found:**
1.
2.
3.

**Corrected YAML:**

```yaml
# Your corrected version here
```

---

### Q8: Incremental Logic for Late-Arriving Data

```sql
{% if is_incremental() %}
-- Your logic here
{% endif %}
```

---

## Part 3: Python

### Q9: Type Hints for Snowflake Query Function

```python
# Your typed version here
```

---

### Q10: Data Validation Function

```python
from typing import Any
import pandas as pd

def validate_order_items(filepath: str) -> dict[str, Any]:
    """
    Validate order_items CSV and return a summary of issues.
    """
    # Your implementation here
    pass
```

---

## Part 4: Pipeline Thinking

### Q11: Handling Late Data / Stale Dashboards

**1. Prevention:**

**2. Proactive Alerts:**

**3. Dashboard Staleness Indicator:**

---

### Q12: Clarifying Questions for LTV Request

1.
2.
3.

---

## Notes / Assumptions

[Add any additional notes or assumptions you made here]
