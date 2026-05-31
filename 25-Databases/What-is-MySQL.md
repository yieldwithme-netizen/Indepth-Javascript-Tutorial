# What is MySQL?

## Definition

MySQL is an open-source relational database management system (RDBMS) that stores data in structured tables with rows and columns. It uses Structured Query Language (SQL) for managing and querying data, and is one of the most popular databases for web applications.

```sql
-- MySQL table structure
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  age INT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Key Concepts

### Tables

Tables are the basic unit of data storage in MySQL, similar to spreadsheets.

```sql
-- Users table
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(50) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  age INT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Posts table
CREATE TABLE posts (
  id INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  content TEXT,
  author_id INT,
  published BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (author_id) REFERENCES users(id)
);
```

### Rows and Columns

```sql
-- Columns define the data structure
-- Rows contain the actual data

-- Insert data
INSERT INTO users (username, email, age) 
VALUES ('alice', 'alice@example.com', 28);

INSERT INTO users (username, email, age) 
VALUES ('bob', 'bob@example.com', 32);

-- Result:
-- | id | username | email             | age | created_at          |
-- |----|----------|-------------------|-----|---------------------|
-- | 1  | alice    | alice@example.com | 28  | 2024-01-15 10:00:00 |
-- | 2  | bob      | bob@example.com   | 32  | 2024-01-15 10:00:00 |
```

## MySQL vs MongoDB

```javascript
// MySQL (Relational)
// Structured schema
// Tables with rows and columns
// SQL queries
// ACID transactions
// Foreign keys for relationships
// Vertical scaling

// MongoDB (NoSQL)
// Flexible schema
// Collections with documents
// JSON-like queries
// eventual consistency (with some ACID support)
// References or embedded documents
// Horizontal scaling
```

## Basic SQL Operations

### CREATE (Insert Data)

```sql
-- Insert single record
INSERT INTO users (username, email, age) 
VALUES ('charlie', 'charlie@example.com', 25);

-- Insert multiple records
INSERT INTO users (username, email, age) VALUES 
  ('dave', 'dave@example.com', 30),
  ('eve', 'eve@example.com', 27);

-- Insert with default values
INSERT INTO posts (title, content, author_id) 
VALUES ('My First Post', 'Hello World!', 1);
```

### READ (Query Data)

```sql
-- Select all users
SELECT * FROM users;

-- Select specific columns
SELECT username, email FROM users;

-- Filter with WHERE clause
SELECT * FROM users WHERE age > 25;

-- Sort results
SELECT * FROM users ORDER BY age DESC;

-- Limit results
SELECT * FROM users LIMIT 10;

-- Join tables
SELECT users.username, posts.title 
FROM users 
JOIN posts ON users.id = posts.author_id;

-- Aggregate functions
SELECT COUNT(*) as total_users FROM users;
SELECT AVG(age) as average_age FROM users;
```

### UPDATE (Modify Data)

```sql
-- Update single record
UPDATE users SET age = 29 WHERE id = 1;

-- Update multiple fields
UPDATE users 
SET age = 29, email = 'alice_new@example.com' 
WHERE id = 1;

-- Update with conditions
UPDATE posts 
SET published = TRUE 
WHERE author_id = 1 AND created_at > '2024-01-01';
```

### DELETE (Remove Data)

```sql
-- Delete single record
DELETE FROM users WHERE id = 1;

-- Delete with conditions
DELETE FROM posts 
WHERE published = FALSE AND created_at < '2024-01-01';

-- Delete all records (use with caution!)
DELETE FROM users;
```

## Relationships

### One-to-One

```sql
-- User profile (one user has one profile)
CREATE TABLE user_profiles (
  user_id INT PRIMARY KEY,
  bio TEXT,
  avatar_url VARCHAR(255),
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### One-to-Many

```sql
-- User has many posts
CREATE TABLE posts (
  id INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(255),
  content TEXT,
  author_id INT,
  FOREIGN KEY (author_id) REFERENCES users(id)
);
```

### Many-to-Many

```sql
-- Users can have many roles, roles can have many users
CREATE TABLE roles (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE user_roles (
  user_id INT,
  role_id INT,
  PRIMARY KEY (user_id, role_id),
  FOREIGN KEY (user_id) REFERENCES users(id),
  FOREIGN KEY (role_id) REFERENCES roles(id)
);
```

## Indexing

```sql
-- Create index for faster queries
CREATE INDEX idx_users_email ON users(email);

-- Unique index
CREATE UNIQUE INDEX idx_users_username ON users(username);

-- Composite index
CREATE INDEX idx_users_name_age ON users(name, age);

-- Show indexes
SHOW INDEX FROM users;
```

## Common Use Cases

- **Web Applications**: User management, content storage
- **E-commerce**: Products, orders, inventory
- **Content Management**: Articles, pages, media
- **Financial Systems**: Transactions, accounts
- **Healthcare**: Patient records, medical data
- **Education**: Student records, courses
- **Social Networks**: User profiles, connections

## Common Mistakes

1. **Not using primary keys**: Always define a primary key for each table
2. **Missing indexes**: Queries on non-indexed columns are slow
3. **SQL injection**: Always use parameterized queries
4. **Not normalizing**: Organize data to reduce redundancy
5. **Ignoring foreign keys**: Maintain referential integrity

```javascript
// WRONG - SQL injection vulnerability
const query = `SELECT * FROM users WHERE email = '${email}'`;
db.query(query);

// CORRECT - Use parameterized queries
const query = 'SELECT * FROM users WHERE email = ?';
db.query(query, [email]);

// WRONG - No index on frequently queried column
SELECT * FROM users WHERE email = 'alice@example.com'; // Slow!

// CORRECT - Create index first
CREATE INDEX idx_users_email ON users(email);
SELECT * FROM users WHERE email = 'alice@example.com'; // Fast!
```

## Quick Revision

- MySQL is a relational database management system (RDBMS)
- Stores data in structured tables with rows and columns
- Uses SQL (Structured Query Language) for queries
- Supports ACID transactions for data integrity
- Tables can be related through foreign keys
- Indexing improves query performance
- Supports various data types (INT, VARCHAR, TEXT, DATE, etc.)
- Vertical scaling (more powerful server) or horizontal scaling (read replicas)
- ACID: Atomicity, Consistency, Isolation, Durability

## Related Topics

- [[What-is-ORM]]
- [[Perform-CRUD]]
- [[Connect-MySQL]]
- [[SQL-Injection]]
- [[Database-Design]]
