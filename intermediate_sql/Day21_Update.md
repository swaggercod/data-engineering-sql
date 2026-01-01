# Day 21 — UPDATE Statement
## 📌 What I Learned Today
Today I learned how to modify existing data in tables using the UPDATE statement. This is essential for maintaining and correcting data.

✅ Basic UPDATE Syntax:
```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```
⚠️ WARNING: Always use WHERE clause! Otherwise, you update ALL rows!

📖 Examples
1. Update Single Row
```sql
-- Update email for specific customer
UPDATE customers
SET email = 'newemail@example.com'
WHERE customer_id = 1;
```
2. Update Multiple Columns
```sql
-- Update both name and age
UPDATE students
SET 
    first_name = 'John',
    age = 17,
    grade = '11th'
WHERE student_id = 5;
```
3. Update Based on Condition
```sql
-- Give raise to all employees in IT department
UPDATE employees
SET salary = salary * 1.10  -- 10% raise
WHERE department = 'IT';
```
4. Update with NULL
```sql
-- Remove phone number (set to NULL)
UPDATE customers
SET phone = NULL
WHERE customer_id = 3;
```
🔍 UPDATE with Different Scenarios:
1. Update Using Calculations
```sql
-- Apply discount to all products over $100
UPDATE products
SET price = price * 0.85  -- 15% discount
WHERE price > 100.00;
```
2. Update with CASE Statement
```sql
-- Different updates based on conditions
UPDATE employees
SET bonus = 
    CASE
        WHEN performance_rating = 'A' THEN 5000
        WHEN performance_rating = 'B' THEN 2500
        ELSE 1000
    END
WHERE year = 2024;
```
3. Update with JOIN
```sql
-- Update based on another table
UPDATE orders o
SET status = 'Shipped'
FROM customers c
WHERE o.customer_id = c.customer_id
AND c.country = 'USA';
```
4. Update with Subquery
```sql
-- Update using result from another query
UPDATE products
SET stock_quantity = 0
WHERE product_id IN (
    SELECT product_id
    FROM sales
    WHERE sale_date < '2024-01-01'
);
```
💡 Real Examples from Sakila-like Tables:
Example 1: Update Customer Information
```sql
-- Change customer's email and address
UPDATE customer
SET 
    email = 'updated@email.com',
    address_id = 5
WHERE customer_id = 10;
```
Example 2: Update Rental Prices
```sql
-- Increase rental rate for popular films
UPDATE film
SET rental_rate = rental_rate * 1.15  -- 15% increase
WHERE film_id IN (
    SELECT film_id
    FROM rental
    GROUP BY film_id
    HAVING COUNT(*) > 50
);
```
Example 3: Fix Data Issues
```sql
-- Correct misspelled category names
UPDATE category
SET name = 'Sci-Fi'
WHERE name = 'SciFi' OR name = 'Science Fiction';
```
⚠️ Common Errors & Solutions:
```sql
-- DANGER: Updates ALL rows (no WHERE clause!)
UPDATE customers SET status = 'inactive';  -- ❌ Updates everyone!

-- Safe: Only updates specific rows
UPDATE customers SET status = 'inactive' 
WHERE last_login < '2023-01-01';  -- ✅

-- ERROR: Invalid column name
UPDATE customers SET emial = 'test@email.com';  -- ❌ Misspelled "email"

-- ERROR: Data type mismatch
UPDATE products SET price = 'free';  -- ❌ Price should be number, not text
```
🎯 Best Practices:
ALWAYS test with SELECT first

```sql
-- First, see what you'll update
SELECT * FROM customers WHERE last_login < '2023-01-01';

-- Then update
UPDATE customers SET status = 'inactive' 
WHERE last_login < '2023-01-01';
```
Use transactions for safety

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```
Limit updates to prevent mistakes

```sql
UPDATE products 
SET discontinued = true 
WHERE product_id = 5
LIMIT 1;  -- Only update 1 row
```
Backup before mass updates

```sql
-- Create backup first
CREATE TABLE customers_backup AS SELECT * FROM customers;
```
🔧 Advanced UPDATE Techniques:
1. UPDATE with RETURNING
```sql
-- Update and see what changed
UPDATE employees
SET salary = salary * 1.05
WHERE department = 'Engineering'
RETURNING employee_id, first_name, salary;
```
2. UPDATE from Another Table
```sql
-- Update using data from another table
UPDATE inventory i
SET stock = stock - s.quantity
FROM sales s
WHERE i.product_id = s.product_id
AND s.sale_date = CURRENT_DATE;
```
3. Conditional UPDATE
```sql
-- Only update if condition is met
UPDATE orders
SET 
    status = 'Completed',
    completed_at = CURRENT_TIMESTAMP
WHERE order_id = 100
AND status = 'Processing';  -- Only update if still processing
```
## ⚡ SQL Commands Quick Reference

| Operation | Syntax | Use Case |
|-----------|--------|----------|
| 🔄 **UPDATE** | `UPDATE table SET col=val WHERE condition` | Modify existing data |
| ➕ **INSERT** | `INSERT INTO table (cols) VALUES (vals)` | Add new records |
| 🗑️ **DELETE** | `DELETE FROM table WHERE condition` | Remove records |
| 📥 **SELECT** | `SELECT cols FROM table WHERE condition` | Retrieve data |

⚡ Pro Tip:
Always follow this pattern:

SELECT to see what will change

BEGIN TRANSACTION

UPDATE with WHERE clause

SELECT to verify changes

ROLLBACK if wrong, COMMIT if correct
