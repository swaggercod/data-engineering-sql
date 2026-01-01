# Day 23 — ALTER Statement
## 📌 What I Learned Today
Today I learned how to modify existing database structures using the ALTER statement. This is essential for maintaining and evolving databases over time.

✅ Basic ALTER Syntax:
```sql
ALTER TABLE table_name
ACTION_TO_TAKE;
📖 Examples
```
1. Add a New Column
```sql
-- Add email column to users table
ALTER TABLE users
ADD COLUMN email VARCHAR(100);

-- Add with constraints
ALTER TABLE employees
ADD COLUMN salary DECIMAL(10,2) NOT NULL DEFAULT 0.00;
```
2. Drop a Column
```sql
-- Remove unwanted column
ALTER TABLE users
DROP COLUMN phone_number;

-- Drop multiple columns
ALTER TABLE products
DROP COLUMN old_price,
DROP COLUMN discount_percent;
```
3. Modify Column
```sql
-- Change data type
ALTER TABLE users
ALTER COLUMN age TYPE INT;

-- Change with constraint
ALTER TABLE orders
ALTER COLUMN order_date SET DEFAULT CURRENT_DATE;
```
4. Rename Things
```sql
-- Rename table
ALTER TABLE old_table_name
RENAME TO new_table_name;

-- Rename column
ALTER TABLE users
RENAME COLUMN user_name TO username;

-- Rename constraint
ALTER TABLE products
RENAME CONSTRAINT pk_prod TO pk_products;
```
🔍 ALTER with Different Scenarios:
1. Add Constraints
```sql
-- Add primary key
ALTER TABLE students
ADD PRIMARY KEY (student_id);

-- Add foreign key
ALTER TABLE orders
ADD FOREIGN KEY (customer_id) REFERENCES customers(customer_id);

-- Add unique constraint
ALTER TABLE users
ADD UNIQUE (email);

-- Add check constraint
ALTER TABLE employees
ADD CHECK (salary >= 0);
```
2. Drop Constraints
```sql
-- Drop constraint by name
ALTER TABLE users
DROP CONSTRAINT unique_email;

-- Drop primary key
ALTER TABLE customers
DROP CONSTRAINT customers_pkey;

-- Drop foreign key
ALTER TABLE orders
DROP CONSTRAINT fk_orders_customers;
```
3. Enable/Disable Constraints
```sql
-- Temporarily disable constraint
ALTER TABLE employees
DISABLE TRIGGER ALL;  -- For data import

-- Re-enable constraint
ALTER TABLE employees
ENABLE TRIGGER ALL;
```
💡 Real Examples from Sakila-like Tables:
Example 1: Add Audit Columns
```sql
-- Add tracking columns to existing table
ALTER TABLE rental
ADD COLUMN created_by VARCHAR(50),
ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
ADD COLUMN update_count INT DEFAULT 0;
```
Example 2: Modify for Performance
```sql
-- Change VARCHAR length for better storage
ALTER TABLE film
ALTER COLUMN title TYPE VARCHAR(150);

-- Add index after table creation
ALTER TABLE payment
ADD INDEX idx_payment_date (payment_date);
```
Example 3: Fix Data Type Issues
```sql
-- Customer phone numbers stored as VARCHAR, need to change
ALTER TABLE customer
ALTER COLUMN phone TYPE VARCHAR(20);
```
⚠️ Common Errors & Solutions:
❌ Error: Column doesn't exist
```sql
ALTER TABLE users
DROP COLUMN middle_name;  -- ERROR if column doesn't exist

-- Solution: Check first
SELECT column_name 
FROM information_schema.columns 
WHERE table_name = 'users';
```
❌ Error: Data type conversion fails
```sql
ALTER TABLE products
ALTER COLUMN price TYPE INTEGER;  -- ERROR if price has decimals

-- Solution: Cast or handle data first
ALTER TABLE products
ALTER COLUMN price TYPE INTEGER
USING ROUND(price::numeric);
```
❌ Error: Constraint violation
```sql
ALTER TABLE users
ADD CONSTRAINT unique_email UNIQUE (email);  -- ERROR if duplicates exist

-- Solution: Clean data first
DELETE FROM users 
WHERE id NOT IN (
    SELECT MIN(id) FROM users GROUP BY email
);
```
🛡️ Safety Best Practices:
1. Always Backup First
```sql
-- Create backup table
CREATE TABLE users_backup AS SELECT * FROM users;

-- Or use transaction
BEGIN;
ALTER TABLE users ADD COLUMN new_column VARCHAR(100);
-- Test, then:
COMMIT;  -- or ROLLBACK if issues
```
2. Test on Development First
Never run ALTER directly on production. Test sequence:

Development → 2. Staging → 3. Production

3. Check Dependencies
```sql
-- See what depends on your table
SELECT 
    tc.table_name, 
    kcu.column_name, 
    ccu.table_name AS foreign_table
FROM information_schema.table_constraints tc
JOIN information_schema.key_column_usage kcu 
    ON tc.constraint_name = kcu.constraint_name
JOIN information_schema.constraint_column_usage ccu 
    ON ccu.constraint_name = tc.constraint_name
WHERE tc.constraint_type = 'FOREIGN KEY' 
AND ccu.table_name = 'your_table';
```
4. Use IF EXISTS / IF NOT EXISTS
```sql
-- Avoid errors if column doesn't exist
ALTER TABLE users
DROP COLUMN IF EXISTS middle_name;

-- Add only if doesn't exist
ALTER TABLE users
ADD COLUMN IF NOT EXISTS email VARCHAR(100);
```
🔧 Advanced ALTER Techniques:
1. Partitioning a Table
```sql
-- Convert to partitioned table
ALTER TABLE sales
PARTITION BY RANGE (sale_date);

-- Add partitions
ALTER TABLE sales
ADD PARTITION sales_2024
FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');
```
2. Change Table Owner
```sql
-- Transfer ownership
ALTER TABLE customers
OWNER TO new_admin;
```
3. Set Table Properties
```sql
-- Add comments
ALTER TABLE employees
COMMENT ON TABLE IS 'Stores employee information';

-- Comment on column
ALTER TABLE employees
COMMENT ON COLUMN salary IS 'Annual salary in USD';
```
4. Alter Sequence
```sql
-- Reset auto-increment counter
ALTER SEQUENCE users_id_seq RESTART WITH 1000;
```
# 📊 Quick Reference:
|Action|	|Syntax|
|----------|------------|
|Add column|	'ALTER TABLE table ADD COLUMN col TYPE'|
|Drop column|	'ALTER TABLE table DROP COLUMN col'|
|Rename column|	'ALTER TABLE table RENAME COLUMN old TO new'|
|Change type|	'ALTER TABLE table ALTER COLUMN col TYPE new_type'|
|Add constraint|	'ALTER TABLE table ADD CONSTRAINT name ...'|
|Drop constraint|	'ALTER TABLE table DROP CONSTRAINT name'|
|Set default|	'ALTER TABLE table ALTER COLUMN col SET DEFAULT value'|
|Drop default|	'ALTER TABLE table ALTER COLUMN col DROP DEFAULT'|
