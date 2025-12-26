# Day 19 — CREATE TABLE
## 📌 What I Learned Today
Today I learned how to create tables in SQL using the CREATE TABLE statement. This is fundamental for setting up database structure.

✅ Basic CREATE TABLE Syntax:
```sql
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    column3 datatype constraints,
    ...
);
```
📖 Examples
1. Simple Table Creation
```sql
-- Create a basic users table
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
2. With All Constraints
```sql
-- Create employees table with various constraints
CREATE TABLE employees (
    emp_id SERIAL PRIMARY KEY,                    -- Auto-incrementing
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    age INT CHECK (age >= 18),                    -- Must be adult
    salary DECIMAL(10,2) CHECK (salary > 0),      -- Positive salary
    hire_date DATE DEFAULT CURRENT_DATE,
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```
3. With Composite Primary Key
```sql
-- Enrollment table (student + course combination)
CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    enrollment_date DATE DEFAULT CURRENT_DATE,
    grade CHAR(2),
    PRIMARY KEY (student_id, course_id),          -- Composite key
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```
4. Copy Table Structure
```sql
-- Create new table with same structure as existing table
CREATE TABLE customers_backup AS 
SELECT * FROM customers WHERE 1=0;  -- Only copies structure, no data
```
💡 Common Data Types:
Data Type	Description	Example
INT / SERIAL	Integer / Auto-increment	user_id SERIAL PRIMARY KEY
VARCHAR(n)	Variable-length string	name VARCHAR(100)
TEXT	Long text	description TEXT
BOOLEAN	True/False	is_active BOOLEAN DEFAULT true
DATE	Date only	birth_date DATE
TIMESTAMP	Date and time	created_at TIMESTAMP
DECIMAL(p,s)	Exact numeric	price DECIMAL(10,2)
JSON / JSONB	JSON data	preferences JSONB
🔧 Table Modifications:
1. Add Column
```sql
ALTER TABLE users 
ADD COLUMN phone VARCHAR(15);
```
2. Drop Column
```sql
ALTER TABLE users 
DROP COLUMN phone;
```
3. Modify Column
```sql
ALTER TABLE users 
ALTER COLUMN email TYPE VARCHAR(150);
```
4. Rename Table
```sql
ALTER TABLE old_name 
RENAME TO new_name;
```
🎯 Best Practices:
Use meaningful names: users, orders, products

Primary key first: Always list PK column first

Use consistent naming: user_id, order_id, product_id

Add constraints: Always define NOT NULL where needed


Document with comments:

```sql
COMMENT ON TABLE users IS 'Stores user account information';
COMMENT ON COLUMN users.email IS 'User email address for login';
```
⚠️ Common Errors:
```sql
-- ERROR: relation "users" already exists
CREATE TABLE users (...);  -- If table already exists

-- ERROR: syntax error
CREATE TABLE users
user_id INT;  -- Missing parentheses

-- Use IF NOT EXISTS to avoid errors
CREATE TABLE IF NOT EXISTS users (...);
```
📊 Real Example from Sakila:
```sql
-- Similar to Sakila's customer table
CREATE TABLE customer (
    customer_id SERIAL PRIMARY KEY,
    store_id INT NOT NULL,
    first_name VARCHAR(45) NOT NULL,
    last_name VARCHAR(45) NOT NULL,
    email VARCHAR(50),
    address_id INT NOT NULL,
    active BOOLEAN NOT NULL DEFAULT true,
    create_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    last_update TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (address_id) REFERENCES address(address_id),
    FOREIGN KEY (store_id) REFERENCES store(store_id)
);
