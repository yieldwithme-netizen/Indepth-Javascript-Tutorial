# Connect to MySQL

## Definition

Connecting to MySQL from JavaScript allows you to interact with relational databases for storing, querying, and managing data. The most common approach is using the `mysql2` package (promise-based) or `mysql` package in Node.js.

## Installation

```bash
# Using npm
npm install mysql2

# Or using yarn
yarn add mysql2
```

## Basic Connection

### Using mysql2 (Promise-based - Recommended)

```javascript
const mysql = require("mysql2/promise");

async function connectToDatabase() {
  try {
    const connection = await mysql.createConnection({
      host: "localhost",
      user: "root",
      password: "your_password",
      database: "your_database",
      port: 3306,
    });

    console.log("Connected to MySQL database!");

    // Test the connection
    const [rows] = await connection.execute("SELECT 1 + 1 AS result");
    console.log("Query result:", rows[0].result); // 2

    await connection.end();
  } catch (error) {
    console.error("Database connection failed:", error.message);
  }
}

connectToDatabase();
```

### Using Connection Pool (Recommended for Production)

```javascript
const mysql = require("mysql2/promise");

const pool = mysql.createPool({
  host: "localhost",
  user: "root",
  password: "your_password",
  database: "your_database",
  port: 3306,
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0,
});

async function queryDatabase() {
  try {
    const [rows] = await pool.execute("SELECT * FROM users");
    console.log("Users:", rows);
  } catch (error) {
    console.error("Query failed:", error.message);
  }
}

queryDatabase();
```

### Callback-based (Legacy)

```javascript
const mysql = require("mysql");

const connection = mysql.createConnection({
  host: "localhost",
  user: "root",
  password: "your_password",
  database: "your_database",
});

connection.connect((err) => {
  if (err) {
    console.error("Connection failed:", err.stack);
    return;
  }
  console.log("Connected as id", connection.threadId);
});

connection.query("SELECT * FROM users", (err, results) => {
  if (err) throw err;
  console.log("Users:", results);
});

connection.end();
```

## CRUD Operations

### Create (INSERT)

```javascript
async function createUser(name, email, age) {
  const [result] = await pool.execute(
    "INSERT INTO users (name, email, age) VALUES (?, ?, ?)",
    [name, email, age]
  );
  console.log("User created with ID:", result.insertId);
  return result.insertId;
}

await createUser("Alice", "alice@example.com", 25);
```

### Read (SELECT)

```javascript
// Get all users
async function getAllUsers() {
  const [rows] = await pool.execute("SELECT * FROM users");
  return rows;
}

// Get user by ID
async function getUserById(id) {
  const [rows] = await pool.execute("SELECT * FROM users WHERE id = ?", [id]);
  return rows[0];
}

// Search with conditions
async function searchUsers(name, minAge) {
  const [rows] = await pool.execute(
    "SELECT * FROM users WHERE name LIKE ? AND age >= ?",
    [`%${name}%`, minAge]
  );
  return rows;
}
```

### Update

```javascript
async function updateUser(id, name, email) {
  const [result] = await pool.execute(
    "UPDATE users SET name = ?, email = ? WHERE id = ?",
    [name, email, id]
  );
  console.log("Rows affected:", result.affectedRows);
  return result.affectedRows > 0;
}

await updateUser(1, "Alice Smith", "alice.smith@example.com");
```

### Delete

```javascript
async function deleteUser(id) {
  const [result] = await pool.execute("DELETE FROM users WHERE id = ?", [id]);
  console.log("Rows deleted:", result.affectedRows);
  return result.affectedRows > 0;
}

await deleteUser(1);
```

## Transactions

```javascript
async function transferMoney(fromId, toId, amount) {
  const connection = await pool.getConnection();

  try {
    await connection.beginTransaction();

    // Deduct from sender
    await connection.execute(
      "UPDATE accounts SET balance = balance - ? WHERE id = ? AND balance >= ?",
      [amount, fromId, amount]
    );

    // Add to receiver
    await connection.execute(
      "UPDATE accounts SET balance = balance + ? WHERE id = ?",
      [amount, toId]
    );

    await connection.commit();
    console.log("Transfer successful");
  } catch (error) {
    await connection.rollback();
    console.error("Transfer failed:", error.message);
    throw error;
  } finally {
    connection.release();
  }
}
```

## Environment Variables

Use `.env` file for sensitive data:

```bash
# .env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=your_database
```

```javascript
require("dotenv").config();
const mysql = require("mysql2/promise");

const pool = mysql.createPool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
});
```

## Common Use Cases

- **Web applications** - User authentication, data storage
- **APIs** - CRUD operations for REST endpoints
- **E-commerce** - Product catalogs, order management
- **Analytics** - Data aggregation and reporting

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| SQL injection via string concatenation | Always use parameterized queries (`?`) |
| Not closing connections | Use connection pools or `finally` blocks |
| Hardcoding credentials | Use environment variables |
| No error handling | Wrap queries in try/catch |
| Creating new connections per query | Use connection pooling |

## Quick Revision

- Use `mysql2/promise` for modern async/await syntax
- **Connection pooling** is essential for production
- Always use **parameterized queries** to prevent SQL injection
- Wrap operations in **try/catch** for error handling
- Use **transactions** for multi-step operations
- Store credentials in **environment variables**

## Related Topics

- [[Node-JS]]
- [[Async-Await]]
- [[Promises]]
- [[CRUD-Operations]]
- [[SQL-Injection]]
- [[Environment-Variables]]
- [[Database-Design]]
