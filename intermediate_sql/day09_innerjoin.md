Day 09 — INNER JOIN
📌 What I Learned Today
Today I started learning about JOINs, specifically the INNER JOIN, which returns only matching rows from both tables.

✅ Key Points:
Used to combine data from two or more tables

Returns rows where the join condition is satisfied in both tables

Most commonly used JOIN type

📖 Examples
Basic INNER JOIN
sql
-- Employees with their departments
SELECT 
    e.name,
    e.salary,
    d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
INNER JOIN with WHERE
sql
-- Employees in 'Engineering' department
SELECT 
    e.name,
    e.salary
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
WHERE d.department_name = 'Engineering';
INNER JOIN with Multiple Conditions
sql
-- Employees with specific criteria
SELECT 
    e.name,
    d.department_name,
    p.project_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
INNER JOIN projects p ON e.id = p.employee_id
WHERE e.salary > 50000;
💡 Notes:
INNER JOIN and JOIN are the same (INNER is optional)

Always specify the join condition with ON

Use table aliases for cleaner code

