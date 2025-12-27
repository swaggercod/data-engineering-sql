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
```
## -QUESTION 6-
How many payments occured on a monday?
```sql
SELECT COUNT(*)
FROM payment
WHERE EXTRACT(dow FROM payment_date) = 1
```
## -QUESTION 7-
How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost?
Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.
```sql
SELECT facid, name, membercost, monthlymaintenance
 FROM cd.facilities
 WHERE membercost > 0 AND
 (membercost < monthlymaintenance/50.0);
```
## -QUESTION 8-
How can you produce a list of members who joined after the start of September 2012?
Return the memid, surname, firstname, and joindate of the members in question.
```sql
SELECT memid, surname, firstname, joindate 
FROM cd.members
 WHERE joindate >= '2012-09-01';
```
## -QUESTION 9-
How can you produce an ordered list of the first 10 surnames in the members table? 
The list must not contain duplicates.
```SQL
SELECT DISTINCT surname
FROM cd.members
ORDER BY  surname LIMIT 10;
```
## -QUESITON 10-
You'd like to get the signup date of your last member. How can you retrieve this information?
```SQL
SELECT MAX(joindate) AS latest_signup
 FROM cd.members;
```
## -QUESTION 11-
Produce a count of the number of facilities that have a cost to guests of 10 or more.
```sql
SELECT COUNT(*) FROM cd.facilities
 WHERE guestcost >= 10;
```
## -QUESTION 12-
Produce a list of the total number of slots booked per facility in the month of September 2012. 
Produce an output table consisting of facility id and slots, sorted by the number of slots.
```sql
SELECT facid, sum(slots) AS "Total Slots"
FROM cd.bookings
 WHERE starttime >= '2012-09-01'
 AND starttime < '2012-10-01'
GROUP BY facid
ORDER BY SUM(slots);
```
## -QUESTION 13-
Produce a list of facilities with more than 1000 slots booked.
Produce an output table consisting of facility id and total slots, sorted by facility id.
```SQL
SELECT facid, sum(slots) AS total_slots
FROM cd.bookings
GROUP BY facid
HAVING SUM(slots) > 1000
ORDER BY facid;
```
## -QUESTION 14-
How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? 
Return a list of start time and facility name pairings, ordered by the time.
```sql
SELECT cd.bookings.starttime AS start, cd.facilities.name 
AS name 
FROM cd.facilities 
INNER JOIN cd.bookings
ON cd.facilities.facid = cd.bookings.facid 
WHERE cd.facilities.facid IN (0,1) 
AND cd.bookings.starttime >= '2012-09-21' 
AND cd.bookings.starttime < '2012-09-22' 
ORDER BY cd.bookings.starttime;
```
## -QUESTION 15-
How can you produce a list of the start times for bookings by members named 'David Farrell'?
```SQL
SELECT cd.bookings.starttime 
FROM cd.bookings 
INNER JOIN cd.members ON 
cd.members.memid = cd.bookings.memid 
WHERE cd.members.firstname='David' 
AND cd.members.surname='Farrell';
```
## -QUESTION 16-
 I have customer data in spreadsheets and need to store it in a database. How do I create a structured table?
 ```sql
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(250) UNIQUE NOT NULL,
    phone VARCHAR(20),
    registration_date TIMESTAMP NOT NULL
);
```
## -QUESTION 17-
 Every time I add a new product, I have to remember the last ID number. How can I automate this?
 ```sql 
 CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    stock_quantity INTEGER NOT NULL,
    category VARCHAR(50)
);

-- When inserting, SERIAL handles the ID automatically
INSERT INTO products (product_name, price, stock_quantity, category)
VALUES ('Laptop', 999.99, 50, 'Electronics');

INSERT INTO products (product_name, price, stock_quantity, category)
VALUES ('Mouse', 29.99, 200, 'Electronics');

-- Product IDs are automatically: 1, 2, 3, etc.
```
## -QUESTION 18-
I have an orders table and a customers table. How do I connect each order to a specific customer?
```sql
-- First, create the parent table (customers)
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(250) UNIQUE NOT NULL
);

-- Then create the child table (orders) with foreign key
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(customer_id),
    order_date TIMESTAMP NOT NULL,
    total_amount DECIMAL(10,2) NOT NULL,
    status VARCHAR(20) NOT NULL
);
```
## -QUESTION 19-
I just got a new customer registration. How do I add their information to the customers table?
```sql
-- Single customer insert
INSERT INTO customers (first_name, last_name, email, phone, registration_date)
VALUES ('Sarah', 'Johnson', 'sarah.j@email.com', '555-0123', CURRENT_TIMESTAMP);

-- Insert and see what was added
INSERT INTO customers (first_name, last_name, email, phone, registration_date)
VALUES ('Mike', 'Chen', 'mike.chen@email.com', '555-0456', CURRENT_TIMESTAMP)
RETURNING *;
```
## -QUESTION 20-
My table has a SERIAL ID column. Do I have to calculate the next ID number myself?
```sql
-- Correct way (let SERIAL handle product_id):
INSERT INTO products (product_name, price, stock_quantity, category)
VALUES ('Laptop', 999.99, 25, 'Electronics');

INSERT INTO products (product_name, price, stock_quantity, category)
VALUES ('Wireless Mouse', 29.99, 150, 'Electronics');

INSERT INTO products (product_name, price, stock_quantity, category)
VALUES ('USB Cable', 9.99, 300, 'Accessories');

-- product_id will be 1, 2, 3 automatically
```
## -QUESTION 21-
I have 10 new employees to add. Do I need to run INSERT 10 times?
```sql
-- No! Insert multiple rows in one command
INSERT INTO employees (first_name, last_name, email, hire_date, salary, department)
VALUES 
    ('John', 'Smith', 'john.s@company.com', '2024-01-15', 75000, 'Engineering'),
    ('Emma', 'Wilson', 'emma.w@company.com', '2024-01-15', 68000, 'Marketing'),
    ('David', 'Brown', 'david.b@company.com', '2024-01-16', 82000, 'Engineering'),
    ('Lisa', 'Davis', 'lisa.d@company.com', '2024-01-16', 71000, 'Sales'),
    ('James', 'Miller', 'james.m@company.com', '2024-01-17', 65000, 'HR'); 
```
## -QUESTION 22-
Customer ID 5 changed their email. How do I update just their record?
```SQL
-- Update single customer's email
UPDATE customers
SET email = 'newemail@example.com'
WHERE customer_id = 5;

-- View the change
SELECT customer_id, first_name, email
FROM customers
WHERE customer_id = 5;
```
## -QUESTION 23-
I want to update one customer's phone number. What if I forget WHERE?
```SQL
-- DANGER! This updates ALL customers (wrong!)
UPDATE customers
SET phone = '555-9999';
-- Every customer now has the same phone number!

-- CORRECT: Use WHERE to target specific customer
UPDATE customers
SET phone = '555-9999'
WHERE customer_id = 10;
-- Only customer 10's phone is updated
```
## -QUESTION 24-
Everyone in the Engineering department gets a 10% raise. How do I update them all?
```SQL
-- Update all engineers' salaries
UPDATE employees
SET salary = salary * 1.10
WHERE department = 'Engineering';

-- Or increase by fixed amount
UPDATE employees
SET salary = salary + 5000
WHERE department = 'Sales';

-- View updated salaries
SELECT first_name, last_name, department, salary
FROM employees
WHERE department = 'Engineering';
```



























