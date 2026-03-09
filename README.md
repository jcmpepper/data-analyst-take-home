# Data Analyst Take-Home Assessment

Thank you for your interest in the Data Analyst role at Pepper.

This assessment evaluates skills directly relevant to our analytics stack: **SQL (Snowflake), dbt, Python, and data pipeline thinking**.

## Time Limit

**1 hour** — we respect your time. This is a hard limit.

We evaluate:
- SQL proficiency (Snowflake dialect, window functions, CTEs, JSON handling)
- dbt fundamentals (models, configurations, incremental strategies)
- Python skills (type hints, data validation, Snowflake connector)
- Pipeline thinking (data quality, automation, stakeholder communication)
- Ability to work with imperfect data

## Dataset

The `data/` folder contains a food-service distribution dataset:

| File | Description | Rows |
|------|-------------|------|
| `customers.csv` | Customer dimension with JSON metadata | 25 |
| `products.csv` | Product catalog with categories | 30 |
| `suppliers.csv` | Supplier information | 10 |
| `orders.csv` | Order headers | 80 |
| `order_items.csv` | Order line items | 156 |
| `promotions.csv` | Active and expired promotions | 5 |

The data contains **intentional quality issues** — inconsistent formatting, missing values, referential integrity problems, and edge cases. Part of the assessment is identifying and handling these.

## Instructions

1. **Fork** this repository to your own GitHub account
2. **Review** the dataset files in `data/`
3. **Complete** the questions in `questions.md`
4. **Submit** your answers in `submissions/[your_name].md`
5. **Share** the link to your completed repo

## Structure

```
├── data/
│   ├── customers.csv
│   ├── products.csv
│   ├── suppliers.csv
│   ├── orders.csv
│   ├── order_items.csv
│   └── promotions.csv
├── questions.md          # Assessment questions
├── submissions/
│   └── [your_name].md    # Your answers go here
└── README.md
```

## What We're Looking For

| Skill | What We Evaluate |
|-------|------------------|
| **SQL** | Correct syntax, efficient queries, proper use of JOINs/window functions/CTEs |
| **dbt** | Understanding of models, configurations, incremental strategies, YAML literacy |
| **Python** | Clean code, proper typing, practical data validation |
| **Data Quality** | Thoroughness in identifying issues, sensible handling strategies |
| **Communication** | Clear explanations, well-organized answers |

## Tips

- **Show your work** — partial credit for good reasoning even if the final answer isn't perfect
- **State assumptions** — if data is ambiguous, explain your interpretation
- **Be concise** — we value clarity over length
- **Don't overthink** — some questions have simple answers

## Questions?

If something is genuinely unclear, make a reasonable assumption and document it. In a real work environment, you'd ask — here, we want to see how you handle ambiguity.

Good luck!
