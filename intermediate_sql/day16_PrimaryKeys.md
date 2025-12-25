# Day 1: Primary Keys in PostgreSQL
## 📌 What I Learned TodayToday 
I learned about primary keys in PostgreSQL, which are fundamental to database design.
A primary key uniquely identifies each row in a table and serves as the main identifier for records.

✅ Key Points:
Primary keys must be unique and non-null
Each table typically has one primary key
They enable table relationships and joins
Serial data type can auto-generate primary key values
Essential for data integrity and record identification
📖 Examples1. Understanding Primary Key Propertiessql-- Viewing a table with primary key
SELECT * FROM customer LIMIT 5;
-- customer_id is marked as PK (Primary Key) in PgAdmin
-- Values: 1, 2, 3, 4, 5 (unique integers, never null)

-- Why first_name can't be a primary key
SELECT first_name, COUNT(*) 
FROM customer 
GROUP BY first_name 
HAVING COUNT(*) > 1;
-- Multiple customers can have the same first name!2. Primary Key in Table Structuresql-- Example of how primary key looks in a customer table
-- customer_id (PK) | first_name | last_name | email
-- 1                | John       | Doe       | john@example.com
-- 2                | Jane       | Smith     | jane@example.com
-- 3                | John       | Wilson    | johnw@example.com

-- Each customer_id is unique and identifies exactly one customer
SELECT customer_id, first_name, last_name
FROM customer
WHERE customer_id = 1;
-- Returns exactly one row3. Primary Key Characteristicssql-- Unique constraint: No duplicate values
SELECT customer_id, COUNT(*) as count
FROM customer
GROUP BY customer_id
HAVING COUNT(*) > 1;
-- Returns 0 rows (no duplicates exist)

-- Non-null constraint: Every row has a value
SELECT COUNT(*) as total_customers,
       COUNT(customer_id) as non_null_ids
FROM customer;
-- Both counts are equal (no nulls)4. Using Primary Keys for Data Retrievalsql-- Find specific customer by primary key (fastest lookup)
SELECT first_name, last_name, email
FROM customer
WHERE customer_id = 15;

-- Primary keys make searches efficient
SELECT customer_id, first_name, last_name
FROM customer
ORDER BY customer_id;5. Primary Keys in Different Tablessql-- Payment table with payment_id as primary key
SELECT payment_id, customer_id, amount, payment_date
FROM payment
LIMIT 5;
-- payment_id: 1, 2, 3, 4, 5 (each payment has unique ID)

-- Film table with film_id as primary key
SELECT film_id, title, rental_rate
FROM film
LIMIT 5;
-- film_id: 1, 2, 3, 4, 5 (each film has unique ID)

-- Staff table with staff_id as primary key
SELECT staff_id, first_name, last_name
FROM staff;
-- staff_id: 1, 2 (each staff member has unique ID)6. Viewing Primary Keys in PgAdminsql-- Any SELECT query will show PK indicator in PgAdmin
SELECT * FROM customer LIMIT 3;
-- Look for the column marked with "PK" label

-- Check table structure programmatically
SELECT column_name, data_type
FROM information_schema.columns
WHERE table_name = 'customer' AND column_name = 'customer_id';
