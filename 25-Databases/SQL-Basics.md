# SQL Basics

## Definition

SQL basics covers **fundamental SQL queries**.

## Commands

```sql
-- Select
SELECT * FROM users;
SELECT name, email FROM users WHERE age > 18;

-- Insert
INSERT INTO users (name, email) VALUES ('John', 'john@example.com');

-- Update
UPDATE users SET name = 'Jane' WHERE id = 1;

-- Delete
DELETE FROM users WHERE id = 1;
```

## Quick Revision

- SELECT: read data
- INSERT: add data
- UPDATE: modify data
- DELETE: remove data

---

## Related Topics

- [[What-is-MySQL]] - [[What-is-MySQL|MySQL]]
- [[What-is-PostgreSQL]] - [[What-is-PostgreSQL|PostgreSQL]]
- [[SQL-Basics]] - [[SQL-Basics|SQL basics]]
- [[What-is-SQL-vs-NoSQL]] - [[What-is-SQL-vs-NoSQL|SQL vs NoSQL]]
