# Primary Keys in SQL - Complete Guide
## 📌 What are Primary Keys?
A Primary Key is a UNIQUE identifier for each row in a database table. Think of it like a student ID number or social security number 
- each one is different and identifies one specific person.

✅ Key Characteristics of Primary Keys:
UNIQUE - No duplicates allowed

NOT NULL - Cannot be empty

Only ONE per table - Each table has just one primary key

Can be single or multiple columns (composite key)

📖 Examples with Sakila Database:
1. Check Existing Primary Keys
```sql
-- See all tables and their primary keys
SELECT 
    tc.table_name,
    kc.column_name
FROM information_schema.table_constraints tc
JOIN information_schema.key_column_usage kc 
    ON tc.constraint_name = kc.constraint_name
WHERE tc.constraint_type = 'PRIMARY KEY'
ORDER BY tc.table_name;
```
2. Common Primary Keys in Sakila:
```sql
-- actor table
SELECT * FROM actor LIMIT 5;
-- actor_id is PRIMARY KEY (1, 2, 3, ...)

-- film table
SELECT * FROM film LIMIT 5;
-- film_id is PRIMARY KEY (1, 2, 3, ...)

-- customer table
SELECT * FROM customer LIMIT 5;
-- customer_id is PRIMARY KEY
🔧 How to Create Primary Keys:
```
Option 1: When Creating Table
```sql
-- Simple primary key
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50)
);

-- With constraint name (better practice)
CREATE TABLE products (
    product_id INT,
    product_name VARCHAR(100),
    price DECIMAL(10,2),
    CONSTRAINT pk_products PRIMARY KEY (product_id)
);

-- Composite primary key (multiple columns)
CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    enrollment_date DATE,
    CONSTRAINT pk_enrollments PRIMARY KEY (student_id, course_id)
);
```
Option 2: Add Primary Key to Existing Table
```sql
-- Add primary key to existing table
ALTER TABLE employees
ADD CONSTRAINT pk_employees PRIMARY KEY (employee_id);

-- Add composite primary key
ALTER TABLE order_items
ADD CONSTRAINT pk_order_items PRIMARY KEY (order_id, product_id);
```
🗑️ How to Remove Primary Key:
```sql
-- Remove primary key constraint
ALTER TABLE table_name
DROP CONSTRAINT pk_name;

-- Example
ALTER TABLE students
DROP CONSTRAINT pk_students;
```
🔍 Find Primary Key of a Specific Table:
```sql
-- Method 1: Using information_schema
SELECT 
    kc.column_name,
    tc.constraint_name
FROM information_schema.table_constraints tc
JOIN information_schema.key_column_usage kc 
    ON tc.constraint_name = kc.constraint_name
WHERE tc.table_name = 'customer'  -- Change table name here
  AND tc.constraint_type = 'PRIMARY KEY';

-- Method 2: Quick check
SELECT 
    a.attname AS column_name
FROM pg_index i
JOIN pg_attribute a ON a.attrelid = i.indrelid
                    AND a.attnum = ANY(i.indkey)
WHERE i.indrelid = 'customer'::regclass  -- Change table name
  AND i.indisprimary;
```
💡 Practical Examples with Sakila:
Example 1: Find actor by Primary Key
```sql
-- Using actor_id (PK) to find specific actor
SELECT * FROM actor WHERE actor_id = 1;
-- This is FAST because actor_id is primary key (indexed)
```
Example 2: Join tables using Primary Keys
```sql
-- actor.actor_id (PK) = film_actor.actor_id (FK)
SELECT 
    a.first_name,
    a.last_name,
    f.title
FROM actor a
JOIN film_actor fa ON a.actor_id = fa.actor_id  -- PK = FK
JOIN film f ON fa.film_id = f.film_id           -- PK = FK
WHERE a.actor_id = 1;
```
Example 3: Check for Duplicate Primary Keys
```sql
-- This should return 0 rows (no duplicates allowed)
SELECT actor_id, COUNT(*) 
FROM actor 
GROUP BY actor_id 
HAVING COUNT(*) > 1;

-- Try to insert duplicate (will fail)
INSERT INTO actor (actor_id, first_name, last_name)
VALUES (1, 'John', 'Doe');  -- ERROR: duplicate key
```
🎯 Primary Key vs Foreign Key:
Primary Key (PK)	Foreign Key (FK)
Identifies rows in current table	References PK in another table
UNIQUE + NOT NULL	Can have duplicates, can be NULL
Only ONE per table	Can have multiple per table
Creates clustered index	Creates non-clustered index
Example relationship:

```sql
-- actor.actor_id is PRIMARY KEY
-- film_actor.actor_id is FOREIGN KEY referencing actor.actor_id
SELECT * FROM film_actor WHERE actor_id = 1;
```
🚀 Best Practices:
Use meaningful names: pk_table_name for constraints

Keep it simple: Use single column PK when possible

Use surrogate keys: Auto-incrementing integers (1, 2, 3...)

Avoid business data: Don't use email, phone as PK (they can change)

Always declare PK: Every table should have one

Good Primary Keys:
user_id (INT, auto-increment)

product_id (UUID for distributed systems)

order_id (BIGINT for large systems)

Bad Primary Keys:
email (can change, too long)

phone_number (can change, not always unique)

first_name (not unique)

📊 Common Primary Key Types:
1. Auto-increment (SERIAL)
```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,  -- Auto: 1, 2, 3...
    username VARCHAR(50)
);
```
2. UUID (Universally Unique)
```sql
CREATE TABLE sessions (
    session_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id INT
);
```
3. Composite Key
```sql
CREATE TABLE grades (
    student_id INT,
    course_id INT,
    grade CHAR(2),
    PRIMARY KEY (student_id, course_id)
);
```
4. Natural Key (rarely used)
```sql
CREATE TABLE countries (
    country_code CHAR(3) PRIMARY KEY,  -- Like 'USA', 'TUR'
    country_name VARCHAR(100)
);
