## -QUESTION 1-
We are launching a platinum service our most loyal customers.
We will assign platinum status to customers that
have 40 or more transaction payments.
What customer ids are eligible for platinum status?

SELECT(customer_id),COUNT(*) FROM payment
GROUP BY customer_id
HAVING COUNT(*) >= 40;

## -QUESTION 2-
What are the customer ids of customers who have spent more than 
100$ in payment transactions with our staff_id member 2?

SELECT(customer_id),SUM(amount)FROM payment
WHERE staff_id = 2
GROUP BY customer_id
HAVING SUM(amount)>100;
