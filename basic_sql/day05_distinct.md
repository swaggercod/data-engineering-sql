Day 5 — DISTINCT
What I learned today

DISTINCT removes duplicate values.

It works on one or multiple columns.

Useful for exploring unique categories or values.
```sql
Notes
-- Unique departments
SELECT DISTINCT department
FROM employees;

-- Unique combinations of departments and job titles
SELECT DISTINCT department, job_title
FROM employees;

When to use DISTINCT

When counting unique values

When cleaning data with duplicates

When checking category lists

Small practice

Get unique employee departments.

Get unique job titles.

Get unique pairs of (department, salary).
