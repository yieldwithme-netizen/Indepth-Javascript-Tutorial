# How to Perform CRUD Operations

## Definition

CRUD stands for Create, Read, Update, and Delete - the four basic operations for managing data in a database. These operations are the foundation of most applications that interact with data.

```javascript
// CRUD Operations
// Create - Insert new data
// Read - Retrieve existing data
// Update - Modify existing data
// Delete - Remove existing data
```

## Using Mongoose (MongoDB)

### Setup

```javascript
const mongoose = require('mongoose');

// Connect to MongoDB
await mongoose.connect('mongodb://localhost:27017/myapp');

// Define schema
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  age: { type: Number, min: 0 },
  isActive: { type: Boolean, default: true },
  createdAt: { type: Date, default: Date.now }
});

// Create model
const User = mongoose.model('User', userSchema);
```

### CREATE - Insert Data

```javascript
// Create single document
const user = new User({
  name: 'Alice Johnson',
  email: 'alice@example.com',
  age: 28
});

await user.save();
console.log('User created:', user);

// Create using create() method
const user = await User.create({
  name: 'Bob Smith',
  email: 'bob@example.com',
  age: 32
});

// Create multiple documents
const users = await User.create([
  { name: 'Charlie', email: 'charlie@example.com', age: 25 },
  { name: 'Dave', email: 'dave@example.com', age: 30 },
  { name: 'Eve', email: 'eve@example.com', age: 27 }
]);

// Insert many (more efficient for large datasets)
const users = await User.insertMany([
  { name: 'Frank', email: 'frank@example.com', age: 35 },
  { name: 'Grace', email: 'grace@example.com', age: 29 }
]);
```

### READ - Query Data

```javascript
// Find all users
const users = await User.find();
console.log('All users:', users);

// Find with conditions
const youngUsers = await User.find({ age: { $lt: 30 } });
console.log('Young users:', youngUsers);

// Find one document
const user = await User.findOne({ email: 'alice@example.com' });
console.log('Found user:', user);

// Find by ID
const user = await User.findById('user_id_here');
console.log('User by ID:', user);

// Select specific fields
const users = await User.find().select('name email');
console.log('Users (name and email only):', users);

// Sort results
const users = await User.find().sort({ age: 1 }); // Ascending
const users = await User.find().sort({ age: -1 }); // Descending

// Limit results
const users = await User.find().limit(5);

// Skip results (pagination)
const page = 2;
const limit = 10;
const users = await User.find()
  .skip((page - 1) * limit)
  .limit(limit);

// Count documents
const count = await User.countDocuments({ isActive: true });

// Chained queries
const users = await User.find({ age: { $gte: 18 } })
  .sort({ name: 1 })
  .limit(10)
  .select('name email age');
```

### UPDATE - Modify Data

```javascript
// Update one document
await User.updateOne(
  { email: 'alice@example.com' },
  { $set: { age: 29 } }
);

// Update by ID and return updated document
const user = await User.findByIdAndUpdate(
  'user_id_here',
  { age: 29 },
  { new: true, runValidators: true }
);

// Update multiple documents
await User.updateMany(
  { age: { $lt: 18 } },
  { $set: { isMinor: true } }
);

// Update with save (triggers middleware)
const user = await User.findById('user_id_here');
user.age = 29;
user.email = 'alice_new@example.com';
await user.save();

// Find one and update
const user = await User.findOneAndUpdate(
  { email: 'alice@example.com' },
  { $inc: { age: 1 } }, // Increment age by 1
  { new: true }
);

// Update operators
await User.updateOne(
  { _id: userId },
  {
    $set: { email: 'newemail@example.com' },
    $push: { tags: 'newTag' },
    $pull: { tags: 'oldTag' },
    $inc: { loginCount: 1 }
  }
);
```

### DELETE - Remove Data

```javascript
// Delete one document
await User.deleteOne({ email: 'alice@example.com' });

// Delete by ID
await User.findByIdAndDelete('user_id_here');

// Delete multiple documents
await User.deleteMany({ age: { $lt: 18 } });

// Delete with conditions
await User.deleteMany({ isActive: false });

// Find one and delete
const user = await User.findOneAndDelete({ email: 'alice@example.com' });

// Remove all documents (use with caution!)
await User.deleteMany({});
```

## Using Raw MongoDB Driver

### Setup

```javascript
const { MongoClient } = require('mongodb');

const uri = 'mongodb://localhost:27017';
const client = new MongoClient(uri);

async function connect() {
  await client.connect();
  return client.db('myapp');
}
```

### CRUD Operations

```javascript
// CREATE
const db = await connect();
const users = db.collection('users');

// Insert one
await users.insertOne({
  name: 'Alice',
  email: 'alice@example.com',
  age: 28
});

// Insert many
await users.insertMany([
  { name: 'Bob', email: 'bob@example.com', age: 32 },
  { name: 'Charlie', email: 'charlie@example.com', age: 25 }
]);

// READ
// Find all
const allUsers = await users.find().toArray();

// Find with filter
const youngUsers = await users.find({ age: { $lt: 30 } }).toArray();

// Find one
const user = await users.findOne({ email: 'alice@example.com' });

// Find by ID
const { ObjectId } = require('mongodb');
const user = await users.findOne({ _id: new ObjectId('user_id_here') });

// UPDATE
// Update one
await users.updateOne(
  { email: 'alice@example.com' },
  { $set: { age: 29 } }
);

// Update many
await users.updateMany(
  { age: { $lt: 18 } },
  { $set: { isMinor: true } }
);

// DELETE
// Delete one
await users.deleteOne({ email: 'alice@example.com' });

// Delete many
await users.deleteMany({ age: { $lt: 18 } });
```

## Using MySQL with mysql2

### Setup

```javascript
const mysql = require('mysql2/promise');

const pool = mysql.createPool({
  host: 'localhost',
  user: 'root',
  password: 'password',
  database: 'myapp',
  waitForConnections: true,
  connectionLimit: 10
});
```

### CRUD Operations

```javascript
// CREATE
const [result] = await pool.execute(
  'INSERT INTO users (name, email, age) VALUES (?, ?, ?)',
  ['Alice', 'alice@example.com', 28]
);
console.log('Inserted ID:', result.insertId);

// READ
const [rows] = await pool.execute('SELECT * FROM users');
console.log('All users:', rows);

const [user] = await pool.execute(
  'SELECT * FROM users WHERE email = ?',
  ['alice@example.com']
);

// UPDATE
const [result] = await pool.execute(
  'UPDATE users SET age = ? WHERE email = ?',
  [29, 'alice@example.com']
);
console.log('Affected rows:', result.affectedRows);

// DELETE
const [result] = await pool.execute(
  'DELETE FROM users WHERE email = ?',
  ['alice@example.com']
);
console.log('Deleted rows:', result.affectedRows);
```

## Common Use Cases

- **User Management**: Create, read, update, delete user accounts
- **Product Catalog**: Manage products in an e-commerce store
- **Blog Posts**: CRUD operations for articles and content
- **Todo Applications**: Task management with create, read, update, delete
- **Inventory Systems**: Track and manage stock items
- **Social Networks**: Posts, comments, and user interactions

## Common Mistakes

1. **Not handling errors**: Always use try-catch for database operations
2. **SQL injection**: Use parameterized queries
3. **Not validating data**: Validate before inserting/updating
4. **Forgetting to close connections**: Always close connections when done
5. **Not using indexes**: Create indexes for frequently queried fields

```javascript
// WRONG - SQL injection vulnerability
const query = `SELECT * FROM users WHERE email = '${email}'`;
await pool.execute(query);

// CORRECT - Use parameterized queries
const [rows] = await pool.execute(
  'SELECT * FROM users WHERE email = ?',
  [email]
);

// WRONG - Not handling errors
const user = await User.findOne({ email });
// What if database connection fails?

// CORRECT - Handle errors
try {
  const user = await User.findOne({ email });
  if (!user) {
    throw new Error('User not found');
  }
} catch (error) {
  console.error('Database error:', error);
}

// WRONG - Not validating data
const user = new User({ email: 'invalid-email' });
await user.save(); // May save invalid data

// CORRECT - Validate data
const user = new User({ email: 'invalid-email' });
try {
  await user.save();
} catch (error) {
  if (error.name === 'ValidationError') {
    console.error('Validation error:', error.message);
  }
}
```

## Quick Revision

- CRUD: Create, Read, Update, Delete
- **Create**: `Model.create()`, `Model.insertMany()`, `new Model().save()`
- **Read**: `Model.find()`, `Model.findOne()`, `Model.findById()`
- **Update**: `Model.updateOne()`, `Model.findByIdAndUpdate()`, `Model.updateMany()`
- **Delete**: `Model.deleteOne()`, `Model.findByIdAndDelete()`, `Model.deleteMany()`
- Always handle errors with try-catch
- Use parameterized queries to prevent SQL injection
- Validate data before database operations
- Create indexes for better query performance
- Close connections when done

## Related Topics

- [[What-is-MongoDB]]
- [[What-is-Mongoose]]
- [[What-is-MySQL]]
- [[What-is-ORM]]
- [[Connect-Mongo]]
