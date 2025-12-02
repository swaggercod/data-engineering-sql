Day 7 — GROUP BY
What I learned today
GROUP BY is used to group rows that have the same values in specified columns into summary rows.

It is almost always used with aggregate functions like COUNT(), SUM(), AVG(), MAX(), and MIN() to perform calculations on each group.

All non-aggregate columns in the SELECT list must appear in the GROUP BY clause.

The HAVING clause is used to filter groups based on a specified condition, unlike the WHERE clause which filters individual rows before grouping.
```sql
Notes
-- Count the total number of employees in each department
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;

-- Calculate the average salary for each job title
SELECT job_title, AVG(salary) AS avg_salary
FROM employees
GROUP BY job_title;

-- Calculate the total salary expenditure for each department,
-- but only show departments with an average salary > 50000
SELECT department, SUM(salary) AS total_salary_expenditure, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
