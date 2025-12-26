# Day 14 — String Functions and Operators in PostgreSQL
## 📌 What I Learned Today
Today I learned about string functions and operators in PostgreSQL, which are essential for text manipulation, data cleaning, and formatting output.

✅ Key Points:
String concatenation for combining text

Case conversion functions for standardization

Substring extraction for getting parts of text

Pattern matching for searching within strings

Trimming and padding for text formatting

📖 Examples
1. String Concatenation (Combining Text)
```sql
-- Using || operator
SELECT 
    first_name || ' ' || last_name AS full_name,
    'Customer ID: ' || customer_id::TEXT AS customer_info
FROM customer
LIMIT 5;

-- Using CONCAT() function
SELECT 
    CONCAT(first_name, ' ', last_name) AS full_name,
    CONCAT(address, ', ', postal_code) AS full_address
FROM customer c
JOIN address a ON c.address_id = a.address_id
LIMIT 5;
```
2. Case Conversion Functions
```sql
-- UPPER, LOWER, INITCAP
SELECT 
    title,
    UPPER(title) AS uppercase_title,
    LOWER(title) AS lowercase_title,
    INITCAP(description) AS propercase_description
FROM film
LIMIT 5;

-- Case-insensitive comparison
SELECT first_name, last_name
FROM customer
WHERE LOWER(last_name) = 'smith';
```
3. String Length and Character Functions
```sql
-- LENGTH and CHAR_LENGTH (same for ASCII)
SELECT 
    title,
    LENGTH(title) AS character_count,
    email,
    LENGTH(email) AS email_length,
    -- Position of '@' in email
    POSITION('@' IN email) AS at_position
FROM customer
LIMIT 5;
```
4. Substring Functions
```sql
-- Extract parts of strings
SELECT 
    title,
    SUBSTRING(title FROM 1 FOR 10) AS first_10_chars,
    LEFT(title, 5) AS first_5_chars,
    RIGHT(title, 5) AS last_5_chars,
    SUBSTRING(title FROM '^[A-Za-z]+') AS first_word
FROM film
LIMIT 5;
```
5. String Replacement and Manipulation
```sql
-- REPLACE function
SELECT 
    title,
    REPLACE(title, 'Action', 'Thriller') AS replaced_title,
    -- Overlay (replace specific part)
    OVERLAY(title PLACING '***' FROM 5 FOR 3) AS masked_title
FROM film
WHERE title LIKE '%Action%'
LIMIT 5;
```
6. Trimming and Padding
```sql
-- Remove whitespace
SELECT 
    TRIM('  hello  ') AS both_trimmed,
    LTRIM('  hello') AS left_trimmed,
    RTRIM('hello  ') AS right_trimmed,
    TRIM(BOTH 'x' FROM 'xxxhelloxxx') AS custom_trim;

-- Add padding
SELECT 
    title,
    LPAD(title, 30, '*') AS left_padded,
    RPAD(title, 30, '-') AS right_padded
FROM film
LIMIT 5;
```
7. Pattern Matching with Regular Expressions
```sql
-- REGEX functions
SELECT 
    title,
    -- Check if matches pattern
    title ~ '^[A-Z]' AS starts_with_capital,
    -- Extract match
    REGEXP_MATCHES(title, '[0-9]+') AS extracted_numbers,
    -- Replace using regex
    REGEXP_REPLACE(title, '[0-9]', '#') AS masked_numbers
FROM film
WHERE title ~ '[0-9]'
LIMIT 5;
💡 Practical Examples
```
Example 1: Data Cleaning and Standardization
```sql
-- Clean customer names and create standardized emails
SELECT 
    customer_id,
    -- Trim and proper case names
    INITCAP(TRIM(BOTH FROM first_name)) AS clean_first_name,
    INITCAP(TRIM(BOTH FROM last_name)) AS clean_last_name,
    -- Create email if missing
    COALESCE(
        email,
        LOWER(CONCAT(
            TRIM(first_name), 
            '.', 
            TRIM(last_name), 
            '@company.com'
        ))
    ) AS email_address,
    -- Extract domain from existing emails
    SUBSTRING(email FROM POSITION('@' IN email) + 1) AS email_domain
FROM customer;
```
Example 2: Address Formatting
```sql
-- Format addresses consistently
SELECT 
    address,
    -- Extract street number and name
    SUBSTRING(address FROM '^[0-9]+') AS street_number,
    SUBSTRING(address FROM '[A-Za-z].+$') AS street_name,
    -- Format phone numbers
    CASE 
        WHEN LENGTH(phone) = 10 THEN 
            '(' || SUBSTRING(phone FROM 1 FOR 3) || ') ' || 
            SUBSTRING(phone FROM 4 FOR 3) || '-' || 
            SUBSTRING(phone FROM 7 FOR 4)
        ELSE phone
    END AS formatted_phone
FROM address
WHERE address_id IN (SELECT address_id FROM customer LIMIT 10);
```
Example 3: Text Analysis
```sql
-- Analyze film descriptions
SELECT 
    title,
    description,
    -- Word count (approximate)
    ARRAY_LENGTH(STRING_TO_ARRAY(description, ' '), 1) AS word_count,
    -- Contains specific keywords
    CASE 
        WHEN description ILIKE '%love%' THEN 'Romance'
        WHEN description ILIKE '%fight%' THEN 'Action'
        WHEN description ILIKE '%murder%' THEN 'Mystery'
        ELSE 'Other'
    END AS genre_hint,
    -- First sentence (up to first period)
    SUBSTRING(description FROM 1 FOR POSITION('.' IN description)) AS first_sentence
FROM film
LIMIT 10;
```
Example 4: Generating Codes and IDs
```sql
-- Generate unique identifiers
SELECT 
    first_name,
    last_name,
    -- Username: first initial + last name
    LOWER(LEFT(first_name, 1) || last_name) AS username,
    -- Customer code: first 3 of last + customer_id
    UPPER(LEFT(last_name, 3) || LPAD(customer_id::TEXT, 4, '0')) AS customer_code,
    -- Password hint (masked)
    CONCAT(
        LEFT(first_name, 1),
        REPEAT('*', LENGTH(last_name) - 2),
        RIGHT(last_name, 1)
    ) AS password_hint
FROM customer
LIMIT 10;
