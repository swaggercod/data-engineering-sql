#Day 08 — HAVING Clause

##What I learned today
The HAVING clause is used to filter groups created by the GROUP BY clause.

It is the only filtering clause that can directly apply conditions to the results of aggregate functions (like COUNT(), SUM(), AVG(), etc.).

It operates after GROUP BY has grouped the rows, unlike the WHERE clause which filters individual rows before grouping.

Notes
```SQL

-- Basic usage: Filter groups where the COUNT(*) is greater than 5
SELECT
    department,
    COUNT(*) AS employee_count
FROM
    employees
GROUP BY
    department
HAVING
    COUNT(*) > 5;

-- Filtering with an aggregate result: Only show departments with an average salary > 50000
SELECT
    department,
    AVG(salary) AS avg_salary
FROM
    employees
GROUP BY
    department
HAVING
    AVG(salary) > 50000;

-- Combining WHERE and HAVING:
-- 1. WHERE filters individual employees (salary > 4000)
-- 2. GROUP BY creates groups
-- 3. HAVING filters the resulting groups (employee_count > 2)
SELECT
    department,
    COUNT(*) AS employee_count
FROM
    employees
WHERE
    salary > 4000
GROUP BY
    department
HAVING
    COUNT(*) > 2;

