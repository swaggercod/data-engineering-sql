## -QUESTION 1-
We are launching a platinum service our most loyal customers.
We will assign platinum status to customers that
have 40 or more transaction payments.
What customer ids are eligible for platinum status?

SELECT(customer_id),COUNT(*) FROM payment
GROUP BY customer_id
HAVING COUNT(*) >= 40;
