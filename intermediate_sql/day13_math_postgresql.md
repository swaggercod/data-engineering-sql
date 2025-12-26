# Day 13 — Mathematical Functions and Operators in PostgreSQL
## 📌 What I Learned Today
Today I learned about mathematical functions and operators in PostgreSQL, which are essential for numerical calculations, data analysis, and business logic.

✅ Key Points:
Arithmetic operators for basic calculations

Mathematical functions for advanced operations

Aggregate functions with math capabilities

Precision control for decimal calculations

📖 Examples
1. Basic Arithmetic Operators
```sql
-- Basic calculations
SELECT 
    10 + 5 AS addition,
    10 - 5 AS subtraction,
    10 * 5 AS multiplication,
    10 / 5 AS division,
    10 % 3 AS modulus, -- Remainder
    10 ^ 3 AS exponentiation; -- 10³ = 1000

-- Using with columns (film rental example)
SELECT 
    film_id,
    title,
    rental_rate,
    rental_rate * 1.1 AS increased_rate, -- 10% increase
    rental_rate / 2 AS half_rate,
    rental_rate + 2.00 AS rate_with_fee
FROM film
LIMIT 10;
```
2. Rounding Functions
```sql
-- Different rounding methods
SELECT 
    12.3456 AS original,
    ROUND(12.3456, 2) AS rounded_2dec, -- 12.35
    ROUND(12.3456) AS rounded_int, -- 12
    CEIL(12.1) AS ceiling, -- 13 (up to nearest integer)
    FLOOR(12.9) AS floor, -- 12 (down to nearest integer)
    TRUNC(12.987, 1) AS truncate_1dec, -- 12.9 (cut off without rounding)
    TRUNC(12.987) AS truncate_int; -- 12
```
3. Power, Roots, and Exponents
```sql
-- Power and roots
SELECT 
    POWER(2, 3) AS two_cubed, -- 8
    POWER(10, 2) AS ten_squared, -- 100
    SQRT(25) AS square_root, -- 5
    CBRT(27) AS cube_root, -- 3
    EXP(1) AS e_constant, -- ~2.718
    LN(2.718) AS natural_log; -- ~1.0
```
4. Trigonometric Functions
```sql
-- Angles in radians (π = PI())
SELECT 
    PI() AS pi_value, -- ~3.14159
    SIN(PI()/2) AS sin_90deg, -- 1
    COS(0) AS cos_0deg, -- 1
    TAN(PI()/4) AS tan_45deg, -- ~1
    DEGREES(PI()) AS pi_to_degrees, -- 180
    RADIANS(180) AS degrees_to_radians; -- π
```
5. Random Numbers and Seeding
```sql
-- Random values (0 to 1)
SELECT 
    RANDOM() AS random_1,
    RANDOM() AS random_2,
    FLOOR(RANDOM() * 100) AS random_0_to_99, -- Integer 0-99
    10 + RANDOM() * 90 AS random_10_to_100; -- Float 10-100

-- Set seed for reproducible random numbers
SELECT SETSEED(0.5);
SELECT RANDOM() AS reproducible_random;
```
6. Absolute Value and Sign
```sql
SELECT 
    ABS(-15) AS absolute_value, -- 15
    ABS(15) AS absolute_positive, -- 15
    SIGN(-10) AS sign_negative, -- -1
    SIGN(
