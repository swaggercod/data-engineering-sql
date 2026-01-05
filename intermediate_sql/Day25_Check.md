# Day 25: PostgreSQL CHECK Constraint
## 📌 What I Learned Today
Today I learned about CHECK constraints in PostgreSQL. They let me add rules to make sure data in tables follows certain conditions. If data doesn't follow the rules, PostgreSQL won't let me insert or update it.

✅ Basic CHECK Constraint Syntax
1. Simple CHECK in CREATE TABLE
```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    age INT CHECK (age >= 18),  -- Age must be 18 or more
    grade CHAR(1) CHECK (grade IN ('A', 'B', 'C', 'D', 'F'))
);
```
2. Multiple CHECK Constraints
```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    price DECIMAL CHECK (price > 0),      -- Price must be positive
    quantity INT CHECK (quantity >= 0),   -- Quantity can't be negative
    category VARCHAR(50) CHECK (category IN ('Electronics', 'Clothing', 'Food'))
);
```
📖 Practical Examples
Example 1: Simple Data Rules
```sql
-- Users table with age and email rules
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(100) CHECK (email LIKE '%@%.%'),  -- Basic email check
    age INT CHECK (age >= 13 AND age <= 120)         -- Reasonable age range
);
```
Example 2: Date Validation
```sql
CREATE TABLE bookings (
    id SERIAL PRIMARY KEY,
    room_number INT,
    check_in DATE,
    check_out DATE,
    CHECK (check_out > check_in)  -- Check-out must be after check-in
);
```
Example 3: Adding CHECK to Existing Table
```sql
-- Add CHECK constraint to existing table
ALTER TABLE employees 
ADD CONSTRAINT salary_check 
CHECK (salary >= 30000);  -- Minimum salary check

-- Another example
ALTER TABLE orders
ADD CONSTRAINT amount_check 
CHECK (total_amount > 0 AND total_amount <= 10000);
```
🔧 Common Use Cases
1. Number Ranges
```sql
CREATE TABLE exams (
    id SERIAL PRIMARY KEY,
    student_id INT,
    score INT CHECK (score >= 0 AND score <= 100)  -- Score between 0-100
);
```
2. List of Valid Values
```sql
CREATE TABLE tasks (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200),
    status VARCHAR(20) CHECK (status IN ('Pending', 'In Progress', 'Completed'))
);
```
3. Multiple Conditions
```sql
CREATE TABLE bank_accounts (
    id SERIAL PRIMARY KEY,
    account_type VARCHAR(20),
    balance DECIMAL,
    CHECK (
        (account_type = 'Savings' AND balance >= 500) OR
        (account_type = 'Checking' AND balance >= 100)
    )
);
```
⚠️ Important Notes
CHECK constraints run for every INSERT and UPDATE

NULL values usually pass CHECK constraints (unless you add NOT NULL)

Use simple conditions - complex checks can slow down performance

Name your constraints for easier management:

```sql
CREATE TABLE example (
    id INT,
    value INT,
    CONSTRAINT value_check CHECK (value > 0)
);
```
💡 When to Use CHECK Constraints
Validate number ranges (age, price, score)

Ensure dates are logical

Restrict to specific values

Basic format validation (like emails)
