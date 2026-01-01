# Day 15 — Subqueries in PostgreSQL
## 📌 What I Learned Today
Today I learned about subqueries (nested queries) in PostgreSQL, which allow me to use the result of one query inside another query. This is essential for complex data analysis and multi-step operations.

✅ Key Points:
Subquery types: Scalar, Row, Table, and Correlated

Different contexts: WHERE, FROM, SELECT, and HAVING clauses

Operators: IN, EXISTS, ANY, ALL for subquery comparisons

Performance considerations: When to use joins vs subqueries

📖 Examples
1. Scalar Subqueries (Return Single Value)
```sql
-- Simple scalar subquery
SELECT 
    title,
    rental_rate,
    (SELECT AVG(rental_rate) FROM film) AS average_rate,
    rental_rate - (SELECT AVG(rental_rate) FROM film) AS difference_from_avg
FROM film
LIMIT 5;

-- In WHERE clause (single value comparison)
SELECT title, rental_rate
FROM film
WHERE rental_rate > (SELECT AVG(rental_rate) FROM film);
```
2. Row Subqueries (Return Single Row)
```sql
-- Compare against multiple columns
SELECT title, rental_rate, length
FROM film
WHERE (rental_rate, length) = (
    SELECT MAX(rental_rate), MIN(length)
    FROM film
);

-- Find customer with highest payment
SELECT first_name, last_name
FROM customer
WHERE customer_id = (
    SELECT customer_id
    FROM payment
    GROUP BY customer_id
    ORDER BY SUM(amount) DESC
    LIMIT 1
);
```
3. Table Subqueries (Return Multiple Rows)
```sql
-- IN operator with subquery
SELECT first_name, last_name
FROM customer
WHERE customer_id IN (
    SELECT customer_id
    FROM rental
    WHERE return_date IS NULL
);

-- ANY operator
SELECT title, rental_rate
FROM film
WHERE rental_rate > ANY (
    SELECT rental_rate
    FROM film
    WHERE rating = 'PG'
);

-- ALL operator (more restrictive)
SELECT title, rental_rate
FROM film
WHERE rental_rate > ALL (
    SELECT rental_rate
    FROM film
    WHERE rating = 'G'
);
```
4. Correlated Subqueries (Reference Outer Query)
```sql
-- Find films with above-average rental rate for their rating
SELECT f1.title, f1.rating, f1.rental_rate
FROM film f1
WHERE f1.rental_rate > (
    SELECT AVG(f2.rental_rate)
    FROM film f2
    WHERE f2.rating = f1.rating
);

-- Customers who have rented more than 10 times
SELECT first_name, last_name
FROM customer c
WHERE (
    SELECT COUNT(*)
    FROM rental r
    WHERE r.customer_id = c.customer_id
) > 10;
```
5. Subqueries in FROM Clause (Derived Tables)
```sql
-- Use subquery as a temporary table
SELECT 
    rating,
    avg_rental_rate,
    film_count
FROM (
    SELECT 
        rating,
        AVG(rental_rate) AS avg_rental_rate,
        COUNT(*) AS film_count
    FROM film
    GROUP BY rating
) AS rating_stats
WHERE film_count > 10;

-- Multiple derived tables
SELECT 
    c.customer_name,
    p.total_payments,
    r.rental_count
FROM (
    SELECT customer_id, 
           CONCAT(first_name, ' ', last_name) AS customer_name
    FROM customer
) c
JOIN (
    SELECT customer_id, SUM(amount) AS total_payments
    FROM payment
    GROUP BY customer_id
) p ON c.customer_id = p.customer_id
JOIN (
    SELECT customer_id, COUNT(*) AS rental_count
    FROM rental
    GROUP BY customer_id
) r ON c.customer_id = r.customer_id;
```
6. EXISTS Operator (Check for Existence)
```sql
-- Customers who have made payments
SELECT first_name, last_name
FROM customer c
WHERE EXISTS (
    SELECT 1
    FROM payment p
    WHERE p.customer_id = c.customer_id
);

-- Films that have never been rented
SELECT title
FROM film f
WHERE NOT EXISTS (
    SELECT 1
    FROM inventory i
    JOIN rental r ON i.inventory_id = r.inventory_id
    WHERE i.film_id = f.film_id
);
