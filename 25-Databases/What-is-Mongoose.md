# What is Mongoose?

## Definition

Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js. It provides a schema-based solution to model application data, including built-in type casting, validation, query building, and business logic hooks.

```javascript
const mongoose = require('mongoose');

// Define a schema
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  age: { type: Number, min: 0, max: 150 }
});

// Create a model
const User = mongoose.model('User', userSchema);
```

## Why Use Mongoose?

### Schema Validation

```javascript
// Without Mongoose - no validation
db.collection('users').insertOne({
  name: "Alice",
  email: "alice@example.com",
  age: "not a number", // No validation!
  invalidField: true   // No schema enforcement!
});

// With Mongoose - automatic validation
const user = new User({
  name: "Alice",
  email: "alice@example.com",
  age: "not a number" // Will fail validation!
});
await user.save(); // Throws validation error
```

### Type Casting

```javascript
// Mongoose automatically casts types
const user = new User({
  name: "Alice",
  email: "alice@example.com",
  age: "28" // String "28" is cast to number 28
});

await user.save();
console.log(user.age); // 28 (number, not string)
```

### Query Building

```javascript
// Build queries with method chaining
const youngUsers = await User.find({ age: { $lt: 30 } })
  .sort({ name: 1 })
  .limit(10)
  .select('name email');

// Mongoose generates the MongoDB query automatically
```

## Defining Schemas

```javascript
const mongoose = require('mongoose');

// Basic schema
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Name is required'],
    trim: true,
    minlength: 2,
    maxlength: 50
  },
  email: {
    type: String,
    required: [true, 'Email is required'],
    unique: true,
    lowercase: true,
    match: [/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/, 'Please enter a valid email']
  },
  age: {
    type: Number,
    min: [0, 'Age cannot be negative'],
    max: [150, 'Age cannot be greater than 150']
  },
  password: {
    type: String,
    required: true,
    minlength: 6,
    select: false // Don't include in queries by default
  },
  role: {
    type: String,
    enum: ['user', 'admin', 'moderator'],
    default: 'user'
  },
  isActive: {
    type: Boolean,
    default: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
});
```

## Schema Types

```javascript
const schema = new mongoose.Schema({
  // String
  name: String,
  
  // Number
  age: Number,
  price: { type: Number, min: 0 },
  
  // Date
  createdAt: Date,
  
  // Boolean
  isActive: Boolean,
  
  // Array
  tags: [String],
  scores: [Number],
  
  // Mixed (any type)
  metadata: mongoose.Schema.Types.Mixed,
  
  // ObjectId (reference to another document)
  author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  
  // Buffer (binary data)
  image: Buffer,
  
  // Decimal128
  balance: mongoose.Schema.Types.Decimal128
});
```

## Creating Models

```javascript
// Define schema
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  age: Number
});

// Create model
const User = mongoose.model('User', userSchema);

// The first argument is the singular name of the collection
// Mongoose automatically pluralizes to 'users'

// Create instance
const user = new User({
  name: 'Alice',
  email: 'alice@example.com',
  age: 28
});

// Save to database
await user.save();
```

## CRUD Operations with Mongoose

### Create

```javascript
// Create single document
const user = new User({
  name: 'Alice',
  email: 'alice@example.com',
  age: 28
});
await user.save();

// Create using create()
const user = await User.create({
  name: 'Bob',
  email: 'bob@example.com',
  age: 32
});

// Create multiple documents
const users = await User.create([
  { name: 'Charlie', email: 'charlie@example.com', age: 25 },
  { name: 'Dave', email: 'dave@example.com', age: 30 }
]);
```

### Read

```javascript
// Find all users
const users = await User.find();

// Find with conditions
const youngUsers = await User.find({ age: { $lt: 30 } });

// Find one document
const user = await User.findOne({ email: 'alice@example.com' });

// Find by ID
const user = await User.findById('user_id_here');

// Select specific fields
const users = await User.find().select('name email');

// Sort results
const users = await User.find().sort({ age: 1 });

// Limit results
const users = await User.find().limit(10);

// Chain methods
const users = await User.find({ age: { $gte: 18 } })
  .sort({ name: 1 })
  .limit(5)
  .select('name email age');
```

### Update

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
await user.save();
```

### Delete

```javascript
// Delete one document
await User.deleteOne({ email: 'alice@example.com' });

// Delete by ID
await User.findByIdAndDelete('user_id_here');

// Delete multiple documents
await User.deleteMany({ age: { $lt: 18 } });
```

## Mongoose Middleware

```javascript
// Pre-save middleware
userSchema.pre('save', function(next) {
  // Hash password before saving
  if (this.isModified('password')) {
    this.password = bcrypt.hashSync(this.password, 10);
  }
  next();
});

// Post-save middleware
userSchema.post('save', function(doc) {
  console.log('User saved:', doc._id);
});

// Pre-find middleware
userSchema.pre('find', function() {
  // Add default conditions
  this.where({ isActive: true });
});
```

## Virtual Properties

```javascript
// Virtual property (not stored in database)
userSchema.virtual('fullName').get(function() {
  return `${this.firstName} ${this.lastName}`;
});

userSchema.virtual('fullName').set(function(name) {
  const parts = name.split(' ');
  this.firstName = parts[0];
  this.lastName = parts[1];
});

// Usage
const user = new User({ firstName: 'Alice', lastName: 'Johnson' });
console.log(user.fullName); // "Alice Johnson"

user.fullName = 'Bob Smith';
console.log(user.firstName); // "Bob"
console.log(user.lastName);  // "Smith"
```

## Common Use Cases

- **Web Applications**: User management, content storage
- **APIs**: Data models for REST/GraphQL APIs
- **Real-time Apps**: Chat messages, notifications
- **E-commerce**: Products, orders, user profiles
- **Social Networks**: Posts, comments, user connections
- **IoT**: Device data, sensor readings

## Common Mistakes

1. **Not awaiting async operations**: Always `await` Mongoose queries
2. **Missing indexes**: Create indexes for frequently queried fields
3. **Not handling validation errors**: Catch and handle validation errors
4. **Overusing populate**: Too many populates can slow queries
5. **Not using select wisely**: Don't return unnecessary fields

```javascript
// WRONG - Not awaiting query
User.find({ age: { $lt: 30 } });
// Returns Query object, not results!

// CORRECT - Await the query
const users = await User.find({ age: { $lt: 30 } });

// WRONG - No error handling
const user = await User.findById(id);
// What if user is null?

// CORRECT - Handle null
const user = await User.findById(id);
if (!user) {
  throw new Error('User not found');
}

// WRONG - Selecting all fields
const users = await User.find();
// Returns all fields including password!

// CORRECT - Select specific fields
const users = await User.find().select('name email');
```

## Quick Revision

- Mongoose is an ODM (Object Data Modeling) library for MongoDB
- Provides schema-based data modeling with validation
- Automatic type casting and default values
- Chainable query methods for building queries
- Supports middleware for pre/post operations
- Virtual properties for computed fields
- Population for referencing related documents
- Indexes for query performance
- Easy CRUD operations with methods like `find()`, `create()`, `updateOne()`, `deleteOne()`

## Related Topics

- [[What-is-MongoDB]]
- [[Connect-Mongo]]
- [[Perform-CRUD]]
- [[What-is-ORM]]
- [[Schema-Design]]
