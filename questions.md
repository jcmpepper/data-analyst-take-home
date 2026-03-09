# Data Analyst Assessment

**Time Limit: 1 hour**

You'll work with a food-service distribution dataset containing orders, customers, products, suppliers, and promotions. This assessment tests skills directly relevant to our analytics stack: SQL (Snowflake), dbt, Python, and pipeline thinking.

---

## Part 1: Data Quality & SQL (25 minutes)

### 1.1 Data Quality Review

Review the provided CSV files (`customers.csv`, `products.csv`, `orders.csv`, `order_items.csv`, `promotions.csv`, `suppliers.csv`).

**Q1:** List all data quality issues you find across the datasets. For each issue, note:
- Which file and field(s) are affected
- What the issue is
- How you would handle it

---

### 1.2 SQL Queries

Write SQL queries (Snowflake dialect) for the following. Assume the CSVs have been loaded into tables with matching names.

**Q2:** Calculate total revenue by product category for **completed orders only**. Include the category name, total revenue, and percentage of overall revenue. Order by revenue descending.

*Expected columns: `category`, `total_revenue`, `pct_of_total`*

---

**Q3:** Find the **top 3 customers by total spend** in each region. Use a window function to rank customers within their region.

*Expected columns: `region`, `customer_name`, `total_spend`, `rank_in_region`*

---

**Q4:** Write a query that identifies customers whose **order frequency increased** in the second half of February compared to the first half. Show the customer name, orders in each period, and the percentage increase.

*Expected columns: `customer_name`, `orders_first_half`, `orders_second_half`, `pct_increase`*

---

**Q5:** Extract the `tier` value from the `metadata` JSON column in the customers table. Then calculate average order value by customer tier.

*Hint: In Snowflake, you can use `metadata:tier::string` or `PARSE_JSON()` + key access*

*Expected columns: `tier`, `avg_order_value`, `order_count`*

---

## Part 2: dbt & Configuration (15 minutes)

### 2.1 Model Configuration

Below is a dbt model configuration. Review it and answer the questions.

```yaml
# models/marts/orders/schema.yml
version: 2

models:
  - name: fct_orders
    description: "Fact table for completed orders with customer and promotion details"
    config:
      materialized: table
      schema: analytics
      tags: ['daily', 'core']
    columns:
      - name: order_id
        description: "Primary key"
        tests:
          - unique
          - not_null
      - name: customer_id
        tests:
          - not_null
          - relationships:
              to: ref('dim_customers')
              field: customer_id
      - name: order_total
        tests:
          - not_null
```

**Q6:** This model is currently materialized as a `table`. If the source data grows to 50M+ rows and we only need to process new/changed records daily, what materialization strategy would you recommend? Explain your reasoning and any configuration changes needed.

---

### 2.2 Debug This Config

The following dbt source configuration has errors. Identify and fix them.

```yaml
# models/staging/sources.yml
version: 2

sources:
  name: raw_data
  database: PRODUCTION_DB
  schema: RAW
  tables:
    - name: orders
      description: Raw orders from source system
      loaded_at_field: created_at
      freshness:
        warn_after: count: 12, period: hour
        error_after: {count: 24, period: hour}
      columns:
        - name: order_id
        - name: order_date
          tests
            - not_null
```

**Q7:** List all errors in the YAML above and provide the corrected version.

---

### 2.3 Incremental Logic

**Q8:** You're building an incremental model that processes order data. The source sometimes sends late-arriving data (orders from 3-4 days ago that weren't in previous loads). Write the `is_incremental()` block that handles this scenario.

```sql
-- models/marts/fct_daily_orders.sql
{{ config(materialized='incremental', unique_key='order_id') }}

SELECT
    order_id,
    customer_id,
    order_date,
    order_total,
    _loaded_at
FROM {{ ref('stg_orders') }}

{% if is_incremental() %}
-- Your logic here
{% endif %}
```

---

## Part 3: Python (15 minutes)

### 3.1 Add Type Hints

The following function connects to Snowflake and runs a query. Add proper type hints to make it pass `mypy --strict`.

```python
# Fix this function with proper type hints
import os

def run_snowflake_query(query, params=None):
    """Execute a query against Snowflake and return results as a list of dicts."""
    import snowflake.connector

    conn = snowflake.connector.connect(
        account=os.environ["SNOWFLAKE_ACCOUNT"],
        user=os.environ["SNOWFLAKE_USER"],
        password=os.environ["SNOWFLAKE_PASSWORD"],
        warehouse="COMPUTE_WH",
        database="ANALYTICS_DB"
    )

    try:
        cursor = conn.cursor()
        if params:
            cursor.execute(query, params)
        else:
            cursor.execute(query)

        columns = [col[0] for col in cursor.description]
        results = []
        for row in cursor.fetchall():
            results.append(dict(zip(columns, row)))
        return results
    finally:
        conn.close()
```

**Q9:** Provide the corrected function with type hints. You may need to import additional types.

---

### 3.2 Data Validation Script

**Q10:** Write a Python function that validates the `order_items.csv` data. The function should:
- Check for missing required fields (`order_id`, `product_id`, `quantity`, `unit_price`)
- Flag negative quantities or prices
- Detect duplicate rows
- Return a summary of all issues found

Use pandas. Include type hints.

```python
from typing import Any
import pandas as pd

def validate_order_items(filepath: str) -> dict[str, Any]:
    """
    Validate order_items CSV and return a summary of issues.

    Returns a dict with keys:
    - 'missing_fields': list of (row_index, field_name) tuples
    - 'negative_values': list of (row_index, field_name, value) tuples
    - 'duplicates': number of duplicate rows
    - 'is_valid': bool indicating if data passed all checks
    """
    # Your implementation here
    pass
```

---

## Part 4: Pipeline Thinking (5 minutes)

**Q11:** You're responsible for a dbt model that calculates daily sales metrics. One morning, you discover the model failed because the source data arrived 6 hours late. The downstream dashboard showed stale data to executives.

Describe how you would:
1. Prevent this from happening again
2. Alert stakeholders proactively when data is delayed
3. Ensure the dashboard indicates when data is stale

---

**Q12:** A business user asks: *"Can you add a new column to the sales report that shows each customer's lifetime value?"*

Before implementing, what questions would you ask to clarify requirements? List at least 3 questions.

---

## Submission

1. Add your answers to `submissions/[your_name].md`
2. For SQL queries, include the full query
3. For Python, include the complete, runnable code
4. For discussion questions, be concise but thorough

**Good luck!**
