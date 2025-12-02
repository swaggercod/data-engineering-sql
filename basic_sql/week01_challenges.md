## -QUESTION 1-
How many transactions were greater than $5.00?

SELECT COUNT (amount) FROM payment
WHERE amount > 5;

## -QUESTION 2-
How many actors have a first name that starts with the "P" letter?

SELECT COUNT (*) FROM actor 
WHERE first_name LIKE 'P%';

## -QUESTION 3-
How many unique districts are our customers from?

SELECT COUNT (DISTINCT(district)) 
FROM address;

## -QUESTION 4-
How many films have a rating of R and a replacement cost between $5 and $15?

SELECT COUNT (rating) FROM film
WHERE rating = 'R' AND replacement_cost BETWEEN 5 AND 15;

## -QUESTION 5-
How many films have the word Truman somewhere in the title?

SELECT COUNT (*) FROM film
WHERE title LIKE '%Truman%';

## -QUESTION 6-
Corporate HQ is conducting a study on the reletionship between replacement cost and a movie MPAA rating 
(e.g G,PG,R etc...).
What is the average replacement cost per MPAA rating?

SELECT rating,AVG(replacement_cost) FROM film
GROUP BY rating;

## -QUESTION 7-
We are running a promotion to reward our top 5 customers with cupons.
What are the coustmer ids of the top 5 customers by total spend?

SELECT customer_id, SUM(amount) FROM payment
GROUP BY customer_id 
ORDER BY SUM (amount) DESC
LIMIT 5


