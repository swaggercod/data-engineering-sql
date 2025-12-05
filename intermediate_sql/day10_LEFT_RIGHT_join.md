# Day 10 — LEFT JOIN & RIGHT JOIN
## 📌 What I Learned Today
Today I learned about LEFT JOIN and RIGHT JOIN, which return all rows from one table and matching rows from the other.

✅ Key Points:
LEFT JOIN: Returns all rows from left table, matched rows from right

RIGHT JOIN: Returns all rows from right table, matched rows from left

NULL values appear when no match is found

📖 Examples
LEFT JOIN and RIGHT JOIN
```sql
-- All employees (even without departments)
SELECT 
    e.name,
    e.salary,
    d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
RIGHT JOIN
sql
-- All departments (even without employees)
SELECT 
    e.name,
    d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;
LEFT JOIN with WHERE
sql
-- Employees without departments
SELECT 
    e.name,
    e.salary
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id
WHERE d.id IS NULL;
