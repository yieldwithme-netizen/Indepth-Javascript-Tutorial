# SQL vs NoSQL

## Definition

**SQL (Structured Query Language)** databases are relational databases that store data in tables with predefined schemas. **NoSQL (Not Only SQL)** databases are non-relational databases that store data in flexible formats like documents, key-value pairs, graphs, or wide-columns.

## Key Differences

| Feature | SQL | NoSQL |
|---------|-----|-------|
| Structure | Tables with rows/columns | Documents, key-value, graph, columnar |
| Schema | Fixed schema | Dynamic/flexible schema |
| Scalability | Vertical (scale up) | Horizontal (scale out) |
| Relationships | Strong ACID compliance | Eventual consistency (BASE) |
| Query Language | SQL | Varies by database |

## SQL Databases

```javascript
// Example: MySQL/PostgreSQL with SQL queries
const mysql = require('mysql2');

const connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'password',
  database: 'myapp'
});

// Create table with fixed schema
connection.execute(`
  CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  )
`);

// SQL query with JOIN
connection.execute(`
  SELECT u.name, o.total
  FROM users u
  JOIN orders o ON u.id = o.user_id
  WHERE o.total > 100
`);
```

## NoSQL Databases (MongoDB)

```javascript
// Example: MongoDB with flexible schema
const { MongoClient } = require('mongodb');

async function connectDB() {
  const client = new MongoClient('mongodb://localhost:27017');
  await client.connect();
  
  const db = client.db('myapp');
  const users = db.collection('users');
  
  // Documents can have different structures
  await users.insertOne({
    name: 'John',
    email: 'john@example.com',
    preferences: { theme: 'dark', language: 'en' }
  });
  
  // No strict schema - can add new fields
  await users.insertOne({
    name: 'Jane',
    email: 'jane@example.com',
    age: 25,
    address: { city: 'NYC', zip: '10001' }
  });
}
```

## When to Use SQL

- Complex queries with multiple JOINs
- Data integrity is critical (banking, e-commerce)
- Structured data with consistent format
- ACID compliance required

## When to Use NoSQL

- Rapid development and prototyping
- Unstructured or semi-structured data
- High scalability requirements
- Real-time applications

## Common Mistakes

1. Using SQL for highly dynamic data structures
2. Using NoSQL when complex relationships exist
3. Ignoring data consistency requirements
4. Not planning for future scalability needs

## Related Topics

- [[What-is-DataModeling]]
- [[What-is-Indexing]]
- [[Use-Sequelize]]

## Quick Revision

- **SQL**: Structured, relational, ACID, vertical scaling
- **NoSQL**: Flexible, non-relational, BASE, horizontal scaling
- Choose SQL for complex queries and data integrity
- Choose NoSQL for flexibility and scalability
