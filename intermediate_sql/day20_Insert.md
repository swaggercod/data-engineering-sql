# Day 20 — INSERT Statement
## 📌 What I Learned Today
Today I learned how to add data to tables using the INSERT statement. This is essential for populating databases with information.

✅ Basic INSERT Syntax:
```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```
📖 Examples
1. Insert Single Row
```sql
-- Insert one customer
INSERT INTO customers (customer_id, name, email, age)
VALUES (1, 'John Doe', 'john@email.com', 30);
```
2. Insert Multiple Rows
```sql
-- Insert multiple customers at once
INSERT INTO customers (customer_id, name, email, age)
VALUES 
    (2, 'Jane Smith', 'jane@email.com', 25),
    (3, 'Bob Wilson', 'bob@email.com', 35),
    (4, 'Alice Brown', 'alice@email.com', 28);
```
3. Insert with Default Values
```sql
-- Use DEFAULT for auto-generated columns
INSERT INTO users (user_id, username, created_at)
VALUES (DEFAULT, 'johndoe', DEFAULT);
```
4. Insert from Another Table
```sql
-- Copy data from one table to another
INSERT INTO customers_backup (customer_id, name, email)
SELECT customer_id, name, email 
FROM customers 
WHERE active = true;
```
🔍 INSERT with Different Scenarios:
1. With SERIAL/Auto-increment
```sql
-- PostgreSQL SERIAL (auto-increment)
INSERT INTO employees (first_name, last_name, salary)
VALUES ('John', 'Doe', 50000);  -- emp_id will auto-generate
```
2. With RETURNING Clause
```sql
-- Get the inserted ID back
INSERT INTO products (name, price)
VALUES ('Laptop', 999.99)
RETURNING product_id;
```
3. Insert with SELECT Subquery
```sql
-- Insert based on query results
INSERT INTO high_value_customers (customer_id, total_spent)
SELECT customer_id, SUM(amount)
FROM payments
GROUP BY customer_id
HAVING SUM(amount) > 1000;
```
💡 Real Examples from Sakila-like Tables:
Example 1: Insert Actor
```sql
INSERT INTO actor (first_name, last_name)
VALUES 
    ('TOM', 'CRUISE'),
    ('MERYL', 'STREEP'),
    ('LEONARDO', 'DICAPRIO');
```
Example 2: Insert Film
```sql
INSERT INTO film (title, description, release_year, rental_rate)
VALUES 
    ('The Matrix', 'A computer hacker learns about reality', 1999, 4.99),
    ('Inception', 'A thief who steals corporate secrets', 2010, 3.99);
```
Example 3: Insert Customer with Address
```sql
INSERT INTO customer (store_id, first_name, last_name, email, address_id)
VALUES 
    (1, 'MICHAEL', 'JOHNSON', 'michael@email.com', 1),
    (1, 'SARAH', 'DAVIS', 'sarah@email.com', 2);
```
⚠️ Common Errors & Solutions:
```sql
-- ERROR: null value in column "email" violates not-null constraint
INSERT INTO users (name) VALUES ('John');  -- Email is required!

-- ERROR: duplicate key value violates unique constraint
INSERT INTO users (email) VALUES ('test@email.com');
INSERT INTO users (email) VALUES ('test@email.com');  -- Same email!

-- ERROR: value too long for type character varying(50)
INSERT INTO users (name) VALUES ('This name is way too long for the column limit');
```
🎯 Best Practices:
Always specify columns - Don't rely on column order

Use transactions for multiple inserts

Validate data before inserting

Use RETURNING to get generated IDs

Batch inserts for better performance

```sql
-- Good: Specify columns
INSERT INTO users (username, email) VALUES ('john', 'john@email.com');

-- Bad: Rely on column order (breaks if table changes)
INSERT INTO users VALUES ('john', 'john@email.com');
```
🔧 Advanced INSERT Techniques:
1. INSERT with ON CONFLICT
```sql
-- Insert or update if exists (UPSERT)
INSERT INTO users (user_id, username, email)
VALUES (1, 'john', 'john@email.com')
ON CONFLICT (user_id) 
DO UPDATE SET email = EXCLUDED.email;
```
2. INSERT with CTE
```sql
-- Using Common Table Expressions
WITH new_customer AS (
    INSERT INTO customers (name, email)
    VALUES ('John Doe', 'john@email.com')
    RETURNING customer_id
)
INSERT INTO orders (customer_id, amount)
SELECT customer_id, 100.00 FROM new_customer;
```
📊 Quick Reference:
Scenario	Syntax
Single row	INSERT INTO table (col1, col2) VALUES (val1, val2)
Multiple rows	INSERT INTO table VALUES (val1), (val2), (val3)
With SELECT	INSERT INTO table SELECT * FROM other_table
Return ID	INSERT ... RETURNING id
Upsert	INSERT ... ON CONFLICT DO UPDATE
