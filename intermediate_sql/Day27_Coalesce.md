# Day 27 — COALESCE in PostgreSQL
## 📌 What I Learned Today
Today I learned about COALESCE() function in PostgreSQL, which returns the first non-NULL value from a list. It's great for handling missing or NULL data.

✅ Basic Syntax:
```sql
COALESCE(value1, value2, value3, ...)
-- Returns first non-NULL value
```
📖 Simple Examples
1. Replace NULL with Default
```sql
-- If email is NULL, use 'No email'
SELECT 
    name,
    COALESCE(email, 'No email') AS contact_email
FROM users;
```
2. Multiple Fallbacks
```sql
-- Try primary phone, then secondary, then 'No phone'
SELECT 
    customer_name,
    COALESCE(phone1, phone2, 'No phone') AS contact_number
FROM customers;
```
3. Calculate with NULL safety
```sql
-- Avoid NULL in calculations
SELECT 
    product,
    price,
    discount,
    price - COALESCE(discount, 0) AS final_price
FROM products;
```
💡 Real PostgreSQL Examples:
Example 1: Customer Contact Info
```sql
-- Get best contact method for customers
SELECT 
    first_name || ' ' || last_name AS customer_name,
    COALESCE(email, phone, 'No contact info') AS contact_method
FROM customer;
```
Example 2: Film Replacement Cost
```sql
-- Use default if replacement cost is missing
SELECT 
    title,
    COALESCE(replacement_cost, 19.99) AS safe_replacement_cost
FROM film;
```
Example 3: Rental Return Date
```sql
-- Show actual return date or 'Not returned yet'
SELECT 
    rental_id,
    rental_date,
    COALESCE(return_date::TEXT, 'Not returned yet') AS return_status
FROM rental;
```
🔍 COALESCE vs CASE:
```sql
-- Same result, different syntax
SELECT 
    name,
    -- With COALESCE
    COALESCE(email, phone, 'No contact') AS contact1,
    -- With CASE
    CASE 
        WHEN email IS NOT NULL THEN email
        WHEN phone IS NOT NULL THEN phone
        ELSE 'No contact'
    END AS contact2
FROM users;
-- Both give same result, but COALESCE is shorter!
```
## 📊 Quick Reference:

| Use Case | Syntax |
|----------|--------|
| Replace NULL | `COALESCE(column, 'default')` |
| Multiple fallbacks | `COALESCE(col1, col2, col3, 'default')` |
| In calculations | `COALESCE(value, 0)` |
| With dates | `COALESCE(date, 'Not set')` |
| With strings | `COALESCE(text, 'Unknown')` |
