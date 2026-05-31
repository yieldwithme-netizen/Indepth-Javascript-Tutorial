# MongoDB

## Definition

MongoDB is a **NoSQL document database** that stores data in JSON-like documents.

## Basic Operations

```javascript
const { MongoClient } = require('mongodb');

// Connect
const client = new MongoClient(uri);
await client.connect();

// Insert
await db.collection('users').insertOne({ name: "John", age: 30 });

// Find
const users = await db.collection('users').find({ age: { $gt: 25 } }).toArray();

// Update
await db.collection('users').updateOne({ name: "John" }, { $set: { age: 31 } });

// Delete
await db.collection('users').deleteOne({ name: "John" });
```

## Quick Revision

- MongoDB = NoSQL document database
- Stores JSON-like documents
- Flexible schema
- Use Mongoose for ODM
- Good for: real-time apps, content management

---

## Related Topics

- [[What-is-MongoDB]] - [[What-is-MongoDB|MongoDB]]
- [[What-is-Mongoose]] - [[What-is-Mongoose|Mongoose]]
- [[Connect-Mongo]] - [[Connect-Mongo|Connecting to MongoDB]]
- [[Perform-CRUD]] - [[Perform-CRUD|CRUD operations]]
