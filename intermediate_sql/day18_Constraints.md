# Day 18 — SQL Constraints
## 📌 What I Learned Today
Constraints are rules enforced on data columns in SQL tables to ensure data accuracy and reliability.

✅ Main Constraint Types:
1. NOT NULL
Ensures a column cannot have NULL values.

```sql
CREATE TABLE users (
    user_id INT NOT NULL,
    email VARCHAR(100) NOT NULL
);
```
2. UNIQUE
Ensures all values in a column are different.

```sql
CREATE TABLE students (
    student_id INT,
    email VARCHAR(100) UNIQUE  -- No duplicate emails allowed
);
```
3. PRIMARY KEY
Combines NOT NULL and UNIQUE. Identifies each row uniquely.

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100)
);
```
4. FOREIGN KEY
Links two tables together.

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```
5. CHECK
Ensures values meet specific conditions.

```sql
CREATE TABLE employees (
    emp_id INT,
    age INT CHECK (age >= 18),  -- Age must be 18+
    salary DECIMAL CHECK (salary > 0)
);
```
6. DEFAULT
Sets a default value when no value is provided.

```sql
CREATE TABLE products (
    product_id INT,
    price DECIMAL DEFAULT 0.00,
    created_date DATE DEFAULT CURRENT_DATE
);
```
💡 Practical Examples:
```sql
-- Complete table with multiple constraints
CREATE TABLE books (
    book_id SERIAL PRIMARY KEY,                    -- Auto-incrementing PK
    title VARCHAR(200) NOT NULL,                   -- Must have title
    isbn VARCHAR(13) UNIQUE,                       -- Unique ISBN
    price DECIMAL(10,2) CHECK (price >= 0),        -- Price can't be negative
    category VARCHAR(50) DEFAULT 'General',        -- Default category
    author_id INT NOT NULL,
    published_date DATE DEFAULT CURRENT_DATE,
    
    FOREIGN KEY (author_id) REFERENCES authors(author_id)
);
```
🔧 Adding/Removing Constraints:
```sql
-- Add constraint to existing table
ALTER TABLE employees
ADD CONSTRAINT chk_age 
CHECK (age >= 18);

-- Remove constraint
ALTER TABLE employees
DROP CONSTRAINT chk_age;

-- Add NOT NULL
ALTER TABLE customers
ALTER COLUMN email SET NOT NULL;

-- Remove NOT NULL
ALTER TABLE customers
ALTER COLUMN email DROP NOT NULL;
```
# SQL Constraints Quick Reference

| Constraint | Purpose | Example |
|------------|---------|---------|
| **NOT NULL** | No empty values | `name VARCHAR(100) NOT NULL` |
| **UNIQUE** | All values different | `email VARCHAR(100) UNIQUE` |
| **PRIMARY KEY** | Unique row identifier | `id INT PRIMARY KEY` |
| **FOREIGN KEY** | Links tables | `FOREIGN KEY (user_id) REFERENCES users(id)` |
| **CHECK** | Validates data | `age INT CHECK (age >= 18)` |
| **DEFAULT** | Sets default value | `created_at TIMESTAMP DEFAULT NOW()` |

---

## **Why Constraints Matter:**

✅ **Data Integrity** - Ensures accurate, reliable data  
✅ **Prevents Invalid Data** - Stops bad data at the database level  
✅ **Documentation** - Shows rules directly in schema  
✅ **Performance** - Some constraints (like PK) improve query speed  

---

## **Common Errors & Solutions:**

```sql
-- ERROR: duplicate key value violates unique constraint
INSERT INTO users (email) VALUES ('test@email.com');
INSERT INTO users (email) VALUES ('test@email.com');  -- ERROR!

-- ERROR: null value violates not-null constraint
INSERT INTO users (name) VALUES (NULL);  -- ERROR!

-- ERROR: check constraint violation
INSERT INTO employees (age) VALUES (16);  -- ERROR if CHECK (age >= 18)
```
⚠️ Common Errors & Solutions:
```sql
-- ERROR: duplicate key value violates unique constraint
INSERT INTO users (email) VALUES ('test@email.com');
INSERT INTO users (email) VALUES ('test@email.com');  -- ERROR!

-- ERROR: null value in column "email" violates not-null constraint
INSERT INTO users (email) VALUES (NULL);  -- ERROR!

-- ERROR: new row violates check constraint
INSERT INTO employees (age) VALUES (16);  -- ERROR if CHECK (age >= 18)
