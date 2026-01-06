# Day 26 — CASE Statement in PostgreSQL
## 📌 What I Learned Today
Today I learned about the CASE statement in PostgreSQL for adding conditional logic to SQL queries.

✅ Basic Syntax:
```sql
-- Simple CASE
CASE column_name
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ELSE default_result
END

-- Searched CASE
CASE 
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE default_result
END
```
📖 Simple Examples
1. Categorize Numbers
```sql
SELECT 
    number,
    CASE 
        WHEN number > 0 THEN 'Positive'
        WHEN number < 0 THEN 'Negative'
        ELSE 'Zero'
    END AS number_type
FROM numbers;
```
2. Grade Students
```sql
SELECT 
    student_name,
    score,
    CASE 
        WHEN score >= 90 THEN 'A'
        WHEN score >= 80 THEN 'B'
        WHEN score >= 70 THEN 'C'
        WHEN score >= 60 THEN 'D'
        ELSE 'F'
    END AS grade
FROM students;
```
3. Status Messages
```sql
SELECT 
    order_id,
    status,
    CASE status
        WHEN 'P' THEN 'Pending'
        WHEN 'S' THEN 'Shipped'
        WHEN 'D' THEN 'Delivered'
        ELSE 'Unknown'
    END AS status_text
FROM orders;
```
💡 Real PostgreSQL Examples:
Example 1: Film Length Categories
```sql
SELECT 
    title,
    length,
    CASE 
        WHEN length < 60 THEN 'Short'
        WHEN length BETWEEN 60 AND 120 THEN 'Medium'
        ELSE 'Long'
    END AS length_category
FROM film;
```
Example 2: Customer Age Groups
```sql
SELECT 
    first_name,
    last_name,
    age,
    CASE 
        WHEN age < 18 THEN 'Minor'
        WHEN age BETWEEN 18 AND 30 THEN 'Young Adult'
        WHEN age BETWEEN 31 AND 50 THEN 'Adult'
        ELSE 'Senior'
    END AS age_group
FROM customer;
```
## 📊 Quick Reference:
Use Case	Syntax
Simple comparison	CASE column WHEN value THEN result END
Multiple conditions	CASE WHEN cond1 THEN res1 WHEN cond2 THEN res2 END
With ELSE	CASE WHEN cond THEN res ELSE default END
In SELECT	SELECT CASE WHEN ... END AS alias
In ORDER BY	ORDER BY CASE WHEN ... END
