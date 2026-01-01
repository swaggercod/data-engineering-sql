# Day 2 — WHERE Clause

## What I learned today
- The WHERE clause is used to filter rows based on a condition.
- It works with numbers, text, comparisons, and logical operators.
- WHERE is one of the most important parts of querying data as a Data Engineer.

## Notes
```sql
-- Basic usage
SELECT * 
FROM employees
WHERE department = 'IT';

-- Multiple conditions
SELECT name, salary
FROM employees
WHERE salary > 4000 AND department = 'Sales';

-- Text filtering
SELECT *
FROM employees
WHERE name LIKE 'A%';

