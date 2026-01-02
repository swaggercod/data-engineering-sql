# Day 24: PostgreSQL DROP Operations
## 📌 What I Learned Today
Today I learned how to safely remove database objects in PostgreSQL using DROP statements. DROP permanently removes database structures, so it must be used carefully with proper precautions.

✅ Key DROP Commands
1. DROP DATABASE - Remove entire database
```sql
-- Safe way with IF EXISTS
DROP DATABASE IF EXISTS old_database;

-- Force drop with active connections
SELECT pg_terminate_backend(pg_stat_activity.pid)
FROM pg_stat_activity 
WHERE datname = 'database_name';
DROP DATABASE database_name;
```
2. DROP TABLE - Remove tables
```sql
-- Basic table drop
DROP TABLE users;

-- Safe drop (won't error if table doesn't exist)
DROP TABLE IF EXISTS temp_data;

-- Drop with dependencies (foreign keys, views)
DROP TABLE orders CASCADE;
```
3. DROP VIEW - Remove views
```sql
DROP VIEW monthly_report;
DROP VIEW IF EXISTS sales_summary;
```
4. DROP INDEX - Remove indexes
```sql
-- Regular drop
DROP INDEX idx_user_email;

-- Concurrent drop (no table lock)
DROP INDEX CONCURRENTLY idx_orders_date;

-- Safe drop
DROP INDEX IF EXISTS unused_index;
```
5. DROP SCHEMA - Remove schemas
```sql
-- Drop empty schema
DROP SCHEMA staging;

-- Drop schema with all objects inside
DROP SCHEMA archive CASCADE;
```
📖 Practical Examples
Example 1: Clean Temporary Objects
```sql
-- Cleanup script
DROP TABLE IF EXISTS temp_logs;
DROP VIEW IF EXISTS temp_report;
DROP INDEX IF EXISTS idx_temp;
```
Example 2: Reset Development Database
```sql
-- Drop all tables in schema
DO $$
DECLARE
    r RECORD;
BEGIN
    FOR r IN (SELECT tablename FROM pg_tables WHERE schemaname = 'public')
    LOOP
        EXECUTE 'DROP TABLE IF EXISTS ' || r.tablename || ' CASCADE';
    END LOOP;
END $$;
```
# ⚠️ Safety Best Practices

**ALWAYS backup before DROP operations**  
**Use IF EXISTS to avoid errors**  
**Test in development before production**  
**Consider CASCADE for dependencies**  
**Use CONCURRENTLY for indexes to avoid locks**

🔄 Common Patterns
```sql
-- Safe object deletion pattern
DROP TABLE IF EXISTS table_name CASCADE;
DROP SEQUENCE IF EXISTS table_name_id_seq;
DROP INDEX IF EXISTS idx_table_column;
```
