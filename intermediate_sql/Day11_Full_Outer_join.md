# Day 11 — FULL OUTER JOIN & CROSS JOIN
## 📌 What I Learned Today
Today I learned about FULL OUTER JOIN and CROSS JOIN, which handle edge cases in joining.

✅ Key Points:
FULL OUTER JOIN: Returns all rows from both tables

CROSS JOIN: Returns Cartesian product (all possible combinations)

Not all SQL databases support FULL OUTER JOIN

📖 Examples
FULL OUTER JOIN and CROSS JOIN
```sql
-- All employees and all departments
SELECT 
    e.name,
    d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```
Finding Disconnected Data
```sql
-- Find employees without departments AND departments without employees
SELECT 
    e.name,
    d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id
WHERE e.id IS NULL OR d.id IS NULL;
```
CROSS JOIN
```sql
-- All possible combinations
SELECT 
    e.name,
    d.department_name
FROM employees e
CROSS JOIN departments d;
