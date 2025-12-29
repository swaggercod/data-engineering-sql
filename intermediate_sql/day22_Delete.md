# Day 22 — DELETE Statement
## 📌 What I Learned Today
Today I learned how to remove data from tables using the DELETE statement. This is essential for data maintenance but must be used carefully!

✅ Basic DELETE Syntax:
```sql
DELETE FROM table_name
WHERE condition;
```
⚠️ WARNING: Without WHERE, you delete ALL rows!

📖 Examples
1. Delete Specific Row
```sql
-- Delete customer with ID 101
DELETE FROM customers
WHERE customer_id = 101;
```
2. Delete Multiple Rows
```sql
-- Delete all inactive customers
DELETE FROM users
WHERE is_active = false;
```
3. Delete Based on Date
```sql
-- Delete orders older than 5 years
DELETE FROM orders
WHERE order_date < CURRENT_DATE - INTERVAL '5 years';
```
4. Delete Using JOIN
```sql
-- Delete customers who have no orders
DELETE FROM customers c
WHERE NOT EXISTS (
    SELECT 1 FROM orders o
    WHERE o.customer_id = c.customer_id
);
```
🔍 DELETE with Different Scenarios:
1. Delete with RETURNING
```sql
-- See what you're deleting
DELETE FROM products
WHERE price < 10
RETURNING product_id, name, price;
```
2. Delete with LIMIT
```sql
-- Delete only 5 oldest records
DELETE FROM log_entries
WHERE id IN (
    SELECT id FROM log_entries
    ORDER BY created_at ASC
    LIMIT 5
);
```
3. Cascade Delete (via Foreign Keys)
```sql
-- If foreign key has ON DELETE CASCADE, related rows auto-delete
-- Example setup:
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id) ON DELETE CASCADE
);
-- Deleting customer will auto-delete their orders
```
💡 Real Examples from Sakila-like Tables:
Example 1: Delete Inactive Customers
```sql
DELETE FROM customer
WHERE active = false
AND last_update < CURRENT_DATE - INTERVAL '1 year';
```
Example 2: Delete Test Data
```sql
DELETE FROM rental
WHERE customer_id IN (
    SELECT customer_id 
    FROM customer 
    WHERE email LIKE '%test%'
);
```
Example 3: Clean Up Old Rentals
```sql
DELETE FROM rental
WHERE rental_date < '2020-01-01'
AND return_date IS NOT NULL;
```
⚠️ DANGER ZONE - Common Mistakes:
❌ NEVER DO THIS:
```sql
-- Deletes EVERYTHING!
DELETE FROM customers;  -- NO WHERE clause!

-- Deletes wrong data
DELETE FROM orders
WHERE total > 100;  -- Double-check your condition!
```
✅ ALWAYS DO THIS:
```sql
-- 1. First, check what will be deleted
SELECT * FROM customers
WHERE last_login < '2023-01-01';

-- 2. Then, delete
DELETE FROM customers
WHERE last_login < '2023-01-01';

-- 3. Use transactions for safety
BEGIN;
DELETE FROM temp_data
WHERE processed = true;
-- Check if correct, then:
COMMIT;
-- Or if wrong:
ROLLBACK;
```
🛡️ Safety Best Practices:
1. Use SELECT First
```sql
-- ALWAYS test with SELECT first
SELECT COUNT(*) FROM users WHERE status = 'inactive';  -- See how many
DELETE FROM users WHERE status = 'inactive';           -- Then delete
```
2. Use Transactions
```sql
BEGIN;
DELETE FROM log WHERE created_at < '2022-01-01';
-- If you made a mistake:
ROLLBACK;
-- If it's correct:
COMMIT;
```
3. Backup First
```sql
-- Create backup before major deletions
CREATE TABLE customers_backup AS 
SELECT * FROM customers WHERE last_login < '2023-01-01';
```
4. Add WHERE Clause LAST
Write your DELETE statement, THEN add the WHERE clause to avoid accidents.

🔧 Advanced DELETE Techniques:
1. Delete Duplicates
```sql
-- Keep only one of duplicate emails
DELETE FROM users u1
WHERE EXISTS (
    SELECT 1 FROM users u2
    WHERE u2.email = u1.email
    AND u2.id < u1.id  -- Keep the smaller ID
);
```
2. Delete with Subquery
```sql
-- Delete customers with no purchases
DELETE FROM customers
WHERE customer_id NOT IN (
    SELECT DISTINCT customer_id FROM orders
);
```
3. Batch Delete for Large Tables
```sql
-- Delete in chunks to avoid locking
DELETE FROM large_table
WHERE id IN (
    SELECT id FROM large_table
    WHERE condition = true
    LIMIT 1000
);
```
## 📊 Quick Reference:

| Scenario | Safe Method |
|----------|------------|
| Delete specific record | `DELETE FROM table WHERE id = 123` |
| Delete old data | `DELETE FROM logs WHERE date < '2023-01-01'` |
| Delete with join | Use subquery: `WHERE id IN (SELECT...)` |
| Delete everything | `TRUNCATE table` (faster) |
| Safe practice | Always use `SELECT` first! |
