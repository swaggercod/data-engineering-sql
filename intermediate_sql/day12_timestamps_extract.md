# Day 12 — Timestamps and EXTRACT
## 📌 What I Learned Today
Today I learned about timestamps and date/time functions in PostgreSQL, which are essential for working with temporal data.

✅ Key Points:
Timestamps store both date and time information

EXTRACT() function retrieves specific parts from timestamps

Date arithmetic allows calculations with dates

Timezone handling is crucial for global applications

📖 Examples
1. Basic Timestamp Functions
```sql
-- Current date and time
SELECT 
    NOW() AS current_timestamp,
    CURRENT_DATE AS current_date,
    CURRENT_TIME AS current_time,
    CURRENT_TIMESTAMP AS current_timestamp_tz;

-- Specific date creation
SELECT 
    DATE '2024-03-15' AS specific_date,
    TIMESTAMP '2024-03-15 14:30:00' AS specific_timestamp,
    TIME '14:30:00' AS specific_time;
2. EXTRACT Function - Getting Date Parts
sql
-- Extract individual components from a timestamp
SELECT 
    rental_date,
    EXTRACT(YEAR FROM rental_date) AS rental_year,
    EXTRACT(MONTH FROM rental_date) AS rental_month,
    EXTRACT(DAY FROM rental_date) AS rental_day,
    EXTRACT(HOUR FROM rental_date) AS rental_hour,
    EXTRACT(MINUTE FROM rental_date) AS rental_minute,
    EXTRACT(SECOND FROM rental_date) AS rental_second
FROM rental
LIMIT 5;

-- Extract day of week (0=Sunday, 6=Saturday)
SELECT 
    EXTRACT(DOW FROM rental_date) AS day_of_week,
    EXTRACT(ISODOW FROM rental_date) AS iso_day_of_week, -- 1=Monday, 7=Sunday
    EXTRACT(DOY FROM rental_date) AS day_of_year
FROM rental
LIMIT 5;
3. DATE_PART Function (Alternative to EXTRACT)
sql
-- DATE_PART does the same thing with different syntax
SELECT 
    rental_date,
    DATE_PART('year', rental_date) AS year,
    DATE_PART('quarter', rental_date) AS quarter,
    DATE_PART('week', rental_date) AS week_number,
    DATE_PART('epoch', rental_date) AS seconds_since_1970
FROM rental
LIMIT 5;
4. Date Arithmetic and Intervals
sql
-- Add/subtract time intervals
SELECT 
    rental_date,
    rental_date + INTERVAL '3 days' AS due_date,
    rental_date - INTERVAL '1 hour' AS one_hour_earlier,
    rental_date + INTERVAL '2 months' AS two_months_later
FROM rental
LIMIT 5;

-- Calculate difference between dates
SELECT 
    rental_date,
    return_date,
    AGE(return_date, rental_date) AS rental_duration,
    return_date - rental_date AS duration_days
FROM rental
WHERE return_date IS NOT NULL
LIMIT 5;
5. Date Formatting with TO_CHAR
sql
-- Format dates in readable ways
SELECT 
    rental_date,
    TO_CHAR(rental_date, 'YYYY-MM-DD') AS simple_date,
    TO_CHAR(rental_date, 'Day, DD Month YYYY') AS full_date,
    TO_CHAR(rental_date, 'HH24:MI:SS') AS time_only,
    TO_CHAR(rental_date, 'YYYY"Q"Q') AS year_quarter,
    TO_CHAR(rental_date, 'WW') AS week_number
FROM rental
LIMIT 5;
6. Real-World Examples
sql
-- 1. Monthly rental statistics
SELECT 
    EXTRACT(YEAR FROM rental_date) AS year,
    EXTRACT(MONTH FROM rental_date) AS month,
    COUNT(*) AS rental_count,
    SUM(amount) AS total_revenue
FROM rental r
JOIN payment p ON r.rental_id = p.rental_id
GROUP BY year, month
ORDER BY year, month;

-- 2. Weekend vs weekday rentals
SELECT 
    CASE 
        WHEN EXTRACT(DOW FROM rental_date) IN (0, 6) THEN 'Weekend'
        ELSE 'Weekday'
    END AS day_type,
    COUNT(*) AS rental_count,
    AVG(amount) AS avg_amount
FROM rental r
JOIN payment p ON r.rental_id = p.rental_id
GROUP BY day_type;

-- 3. Films rented in the last 30 days
SELECT 
    f.title,
    r.rental_date,
    AGE(NOW(), r.rental_date) AS time_ago
FROM film f
JOIN inventory i ON f.film_id = i.film_id
JOIN rental r ON i.inventory_id = r.inventory_id
WHERE r.rental_date > CURRENT_DATE - INTERVAL '30 days'
ORDER BY r.rental_date DESC;

-- 4. Time-based customer segmentation
SELECT 
    EXTRACT(HOUR FROM rental_date) AS rental_hour,
    COUNT(DISTINCT customer_id) AS unique_customers,
    COUNT(*) AS total_rentals
FROM rental
GROUP BY rental_hour
ORDER BY rental_hour;
