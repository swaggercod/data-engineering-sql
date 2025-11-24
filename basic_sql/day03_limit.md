# Day 3 — LIMIT Clause

## What I learned today
- The LIMIT clause is used to control how many rows are returned.
- It helps preview data, test queries faster, and avoid scanning large datasets.
- LIMIT is especially useful when working with big tables in data engineering.

## Notes
```sql
-- Return first 5 rows
SELECT *
FROM employees
LIMIT 5;

-- Return first 10 rows with specific columns
SELECT name, department
FROM employees
LIMIT 10;

-- Combine with WHERE
SELECT *
FROM employees
WHERE salary > 5000
LIMIT 3;
