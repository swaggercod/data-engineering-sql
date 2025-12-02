# Day 4 — ORDER BY

## What I learned today
 ORDER BY is used to sort query results.
 Default order is ASC (ascending).
 Use DESC for descending order.
 ORDER BY can sort by one or multiple columns.

## Notes
```sql
-- Sort employees by salary (low to high)
SELECT *
FROM employees
ORDER BY salary ASC;

-- Sort employees by salary (high to low)
SELECT *
FROM employees
ORDER BY salary DESC;

-- Sort by department first, then salary
SELECT name, department, salary
FROM employees
ORDER BY department ASC, salary DESC;
