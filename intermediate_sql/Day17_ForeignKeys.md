# Day 17 — Foreign Keys 
## 📌 What is a Foreign Key?
A column in one table that references another table's Primary Key

Creates relationships between tables

## Primary Key vs Foreign Key:

| Primary Key (PK) | Foreign Key (FK) |
|-----------------|------------------|
| Identifies rows in **current** table | References PK in **another** table |
| **UNIQUE + NOT NULL** | Can have duplicates, can be NULL |
| Only **ONE per table** | Can have **multiple** per table |
| Creates **clustered index** | Creates **non-clustered index** |
| `actor.actor_id` | `film_actor.actor_id` |

**Example relationship:**
```sql
-- actor.actor_id is PRIMARY KEY
-- film_actor.actor_id is FOREIGN KEY referencing actor.actor_id
SELECT * FROM film_actor WHERE actor_id = 1;
🔍 Examples from Sakila:
```sql
-- film_actor has TWO foreign keys
SELECT *
FROM film_actor
WHERE actor_id = 1;  -- FK to actor.actor_id

-- See the relationship
SELECT a.first_name, f.title
FROM actor a
JOIN film_actor fa ON a.actor_id = fa.actor_id  -- FK = PK
JOIN film f ON fa.film_id = f.film_id           -- FK = PK
WHERE a.actor_id = 1;
```
🔧 How to Create:
```sql
-- When creating table
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

-- Add to existing table
ALTER TABLE payments
ADD FOREIGN KEY (customer_id) REFERENCES customers(customer_id);
```
⚡ Key Points:
Ensures data integrity - Can't reference non-existent data

Creates relationships - Links tables logically

Optional - Can be NULL

Multiple FKs in one table allowed

Must match data type exactly with referenced PK

💡 Simple Rule:
Foreign Key always points to a Primary Key in another table.

Example Flow:
payment.customer_id → customer.customer_id
(FK in payment table) → (PK in customer table)
