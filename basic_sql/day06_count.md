# Day 6 — COUNT()
## What I learned today

COUNT() is used to count rows.

COUNT(column) ignores NULL values.

COUNT(*) counts all rows.

Often used with DISTINCT and GROUP BY.
```sql
Notes
-- Count all employees
SELECT COUNT(*)
FROM employees;

-- Count non-null salaries
SELECT COUNT(salary)
FROM employees;

-- Count unique departments
SELECT COUNT(DISTINCT department)
FROM employees;

Tips

COUNT(*) is the safest option.

COUNT(column) = ignores NULLs.

COUNT(DISTINCT x) = unique count.

Small practice

Count total number of employees.

Count how many employees work in IT.

Count unique departments.

Count employees with salary > 4000.
