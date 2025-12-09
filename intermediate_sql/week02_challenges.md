## -QUESTION 1-
We are launching a platinum service our most loyal customers.
We will assign platinum status to customers that have 
40 or more transaction payments.
What customer ids are eligible for platinum status?
```sql  
SELECT(customer_id),COUNT(*) FROM payment
GROUP BY customer_id
HAVING COUNT(*) >= 40;
```
## -QUESTION 2-
What are the customer ids of customers who have spent more than 
100$ in payment transactions with our staff_id member 2?
```sql
SELECT(customer_id),SUM(amount)FROM payment
WHERE staff_id = 2
GROUP BY customer_id
HAVING SUM(amount)>100;
```
## -QUESTION 3-
What are the emails of the customers who live in California?
```sql
SELECT email,district FROM address
INNER JOIN customer ON 
address.address_id = customer.address_id
WHERE district = 'California'
```
## -QUESTION 4-
A customer walks in and is a huge fan of the actor "Nick Wahlberg"
and wants to kmow which movies he is in.
Get a list of all the movies "Nick Wahlberg" has been in.
```sql
SELECT title,first_name,last_name FROM film_actor
INNER JOIN actor ON film_actor.actor_id = actor.actor_id
INNER JOIN film ON film_actor.film_id = film.film_id
WHERE first_name = 'Nick'
AND last_name = 'Wahlberg'
```
## -QUESTION 5-
During which months did payments occur?
Format your answer to return back the full month name.
```sql
SELECT DISTINCT(TO_CHAR(payment_date,'MONTH'))
FROM payment
