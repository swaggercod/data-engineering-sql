# Day 1: Primary Keys
## What is a Primary Key?
A primary key is a column or group of columns used to uniquely identify a row in a table.
Key Characteristics

Unique: Must be distinct for every row - no two rows can have the same primary key value
Non-null: Must always have a value, cannot be empty
Single identifier: Each table typically has one primary key that serves as the main identifier

Why Primary Keys Matter

They uniquely identify each record in your database
They help determine which columns to use when joining tables together
They ensure data integrity by preventing duplicate records

Real-World Example
In a DVD rental database's customer table:

```sql customer_id``` is the primary key
Each customer gets a unique ID number (1, 2, 3, etc.)
You can't use "first_name" as a primary key because multiple people can share the same name
But a unique integer ID ensures each customer record is distinct

Technical Note
PostgreSQL offers a ```sql serial``` data type that automatically generates unique integer values for primary keys as you add new rows to a table.
Viewing Primary Keys in PgAdmin

Run a SELECT query on any table
Primary keys are marked with "PK" label
Gold key symbol indicates a primary key in the table structure view
