# What is MongoDB?

## Definition

MongoDB is a NoSQL, document-oriented database that stores data in flexible, JSON-like documents. Unlike traditional relational databases that store data in tables and rows, MongoDB stores data in collections and documents, making it highly scalable and flexible.

```javascript
// MongoDB document example
{
  _id: ObjectId("507f1f77bcf86cd799439011"),
  name: "Alice Johnson",
  email: "alice@example.com",
  age: 28,
  address: {
    street: "123 Main St",
    city: "New York",
    state: "NY",
    zip: "10001"
  },
  hobbies: ["reading", "hiking", "coding"],
  createdAt: new Date("2024-01-15")
}
```

## Key Concepts

### Documents

Documents are the basic unit of data in MongoDB, similar to rows in relational databases. They are stored in BSON format (Binary JSON).

```javascript
// Simple document
{
  _id: ObjectId("507f1f77bcf86cd799439011"),
  title: "My First Post",
  content: "This is the content of my post",
  author: "Alice",
  published: true,
  tags: ["javascript", "nodejs"],
  comments: [
    { user: "Bob", text: "Great post!", date: new Date() },
    { user: "Charlie", text: "Thanks for sharing", date: new Date() }
  ]
}
```

### Collections

Collections are groups of documents, similar to tables in relational databases.

```javascript
// Collections in MongoDB
// - users
// - posts
// - comments
// - products
// - orders
```

### Database

A database is a container for collections.

```javascript
// Database examples
// - myapp (development)
// - myapp_prod (production)
```

## MongoDB vs SQL Databases

```javascript
// SQL Database (MySQL, PostgreSQL)
// Table: users
// | id | name  | email             | age |
// |----|-------|-------------------|-----|
// | 1  | Alice | alice@example.com | 28  |
// | 2  | Bob   | bob@example.com   | 32  |

// MongoDB
// Collection: users
[
  {
    _id: ObjectId("..."),
    name: "Alice",
    email: "alice@example.com",
    age: 28
  },
  {
    _id: ObjectId("..."),
    name: "Bob",
    email: "bob@example.com",
    age: 32
  }
]
```

## MongoDB Features

### Flexible Schema

```javascript
// Documents in the same collection can have different structures
// User 1
{
  _id: ObjectId("..."),
  name: "Alice",
  email: "alice@example.com"
}

// User 2 (has additional fields)
{
  _id: ObjectId("..."),
  name: "Bob",
  email: "bob@example.com",
  age: 32,
  phone: "555-1234",
  preferences: {
    newsletter: true,
    notifications: false
  }
}
```

### Rich Query Language

```javascript
// Find users
db.users.find({ age: { $gte: 18 } });

// Update user
db.users.updateOne(
  { _id: ObjectId("...") },
  { $set: { age: 29 } }
);

// Delete user
db.users.deleteOne({ _id: ObjectId("...") });

// Aggregation pipeline
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: { _id: "$userId", total: { $sum: "$amount" } } }
]);
```

### Indexing

```javascript
// Create index for faster queries
db.users.createIndex({ email: 1 }, { unique: true });
db.users.createIndex({ name: 1, age: -1 });
```

## MongoDB Data Types

```javascript
// String
"name": "Alice"

// Number (integer)
"age": 28

// Number (floating point)
"price": 19.99

// Boolean
"active": true

// Array
"hobbies": ["reading", "hiking"]

// Object (nested document)
"address": {
  "city": "New York",
  "state": "NY"
}

// Date
"createdAt": ISODate("2024-01-15T00:00:00Z")

// ObjectId (unique identifier)
"_id": ObjectId("507f1f77bcf86cd799439011")

// Null
"middleName": null

// Binary Data
"profileImage": BinData(0, "...")

// Regular Expression
"pattern": /test/i
```

## Common Use Cases

- **Content Management Systems**: Store articles, pages, and media
- **E-commerce**: Products, orders, and user profiles
- **Real-time Analytics**: Event tracking and analytics data
- **IoT Applications**: Sensor data and device information
- **Mobile Apps**: User data and app state
- **Gaming**: Player data and game state
- **Social Networks**: Posts, comments, and user connections

## Common Mistakes

1. **Not using ObjectId correctly**: Always use `ObjectId()` for _id queries
2. **Ignoring indexes**: Queries without indexes are slow
3. **Over-normalizing**: MongoDB is not relational - denormalize when needed
4. **Not handling connections**: Always close database connections properly
5. **Ignoring data validation**: Use schema validation even with flexible schema

```javascript
// WRONG - Querying _id as string
db.users.find({ _id: "507f1f77bcf86cd799439011" });

// CORRECT - Querying _id as ObjectId
db.users.find({ _id: ObjectId("507f1f77bcf86cd799439011") });

// WRONG - No index on frequently queried field
db.users.find({ email: "alice@example.com" }); // Slow!

// CORRECT - Create index first
db.users.createIndex({ email: 1 });
db.users.find({ email: "alice@example.com" }); // Fast!
```

## Quick Revision

- MongoDB is a NoSQL, document-oriented database
- Stores data in JSON-like documents (BSON format)
- Documents are stored in collections (like tables)
- Each document has a unique `_id` field (ObjectId)
- Supports flexible schema - documents can have different structures
- Rich query language with CRUD operations and aggregation
- Indexing improves query performance
- ACID transactions available for multi-document operations
- Horizontal scaling through sharding

## Related Topics

- [[What-is-Mongoose]]
- [[Connect-Mongo]]
- [[Perform-CRUD]]
- [[What-is-ORM]]
- [[Nodejs-Modules]]
