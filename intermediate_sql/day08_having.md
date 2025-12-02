
Day 08 Summary: The HAVING Clause (PostgreSQL/SQL)
The HAVING clause is a fundamental tool in SQL used specifically for filtering groups that have been created using the GROUP BY clause. It is the primary way to apply conditions to the results of aggregate functions like SUM(), AVG(), or COUNT().

 Key Function and Distinction
The most important concept of HAVING is its distinction from the WHERE clause:

WHERE filters individual rows before they are grouped. It cannot use aggregate functions.

HAVING filters summary rows (groups) after the grouping and aggregation are complete. It can use aggregate functions.

 SQL Clause Execution Order
The logical order in which the database executes a query clarifies the role of HAVING :

FROM

WHERE (Individual row filtering)

GROUP BY (Row combination)

HAVING (Group filtering)

SELECT

ORDER BY

 Example Use Case
The HAVING clause is essential when you need to pose a question about the summarized data.

Goal: Calculate the average salary for every job title, but only display the job titles where the average salary is greater than $70,000.

```SQL

SELECT
    job_title,
    AVG(salary) AS avg_salary
FROM
    employees
GROUP BY
    job_title
HAVING 
    AVG(salary) > 70000;
