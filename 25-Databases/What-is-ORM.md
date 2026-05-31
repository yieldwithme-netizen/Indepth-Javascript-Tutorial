# What is an ORM?

## Definition

ORM (Object-Relational Mapping) is a technique that converts data between incompatible type systems in object-oriented programming languages and relational databases. It allows you to interact with a database using objects and methods instead of writing raw SQL queries.

```javascript
// Without ORM - Raw SQL
const query = 'SELECT * FROM users WHERE age > ?';
const users = await db.execute(query, [18]);

// With ORM - Object-oriented approach
const users = await User.find({ age: { $gt: 18 } });
```

## How ORM Works

```javascript
// ORM maps database tables to classes
class User {
  constructor(name, email, age) {
    this.name = name;
    this.email = email;
    this.age = age;
  }
}

// ORM maps table rows to objects
const user = new User('Alice', 'alice@example.com', 28);

// ORM provides methods for CRUD operations
await user.save(); // INSERT
await User.findById(1); // SELECT
await user.update({ age: 29 }); // UPDATE
await user.delete(); // DELETE
```

## Popular ORMs in JavaScript

### Mongoose (MongoDB ODM)

```javascript
const mongoose = require('mongoose');

// Define schema
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  age: { type: Number, min: 0 }
});

// Create model
const User = mongoose.model('User', userSchema);

// CRUD Operations
// Create
const user = new User({ name: 'Alice', email: 'alice@example.com', age: 28 });
await user.save();

// Read
const users = await User.find({ age: { $gt: 18 } });

// Update
await User.updateOne({ email: 'alice@example.com' }, { $set: { age: 29 } });

// Delete
await User.deleteOne({ email: 'alice@example.com' });
```

### Sequelize (SQL ORM)

```javascript
const { Sequelize, DataTypes } = require('sequelize');

// Create connection
const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'mysql'
});

// Define model
const User = sequelize.define('User', {
  name: {
    type: DataTypes.STRING,
    allowNull: false
  },
  email: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true,
    validate: {
      isEmail: true
    }
  },
  age: {
    type: DataTypes.INTEGER,
    validate: {
      min: 0,
      max: 150
    }
  }
}, {
  timestamps: true
});

// Sync model with database
await User.sync();

// CRUD Operations
// Create
const user = await User.create({ 
  name: 'Alice', 
  email: 'alice@example.com', 
  age: 28 
});

// Read
const users = await User.findAll({ where: { age: { [Op.gt]: 18 } } });
const user = await User.findByPk(1);

// Update
await User.update({ age: 29 }, { where: { email: 'alice@example.com' } });

// Delete
await User.destroy({ where: { email: 'alice@example.com' } });
```

### TypeORM (TypeScript ORM)

```typescript
import { Entity, PrimaryGeneratedColumn, Column, createConnection } from 'typeorm';

// Define entity
@Entity()
class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column({ unique: true })
  email: string;

  @Column({ nullable: true })
  age: number;
}

// Create connection
const connection = await createConnection({
  type: 'mysql',
  host: 'localhost',
  port: 3306,
  username: 'root',
  password: 'password',
  database: 'myapp',
  entities: [User],
  synchronize: true
});

// Get repository
const userRepository = connection.getRepository(User);

// CRUD Operations
// Create
const user = new User();
user.name = 'Alice';
user.email = 'alice@example.com';
user.age = 28;
await userRepository.save(user);

// Read
const users = await userRepository.find({ where: { age: MoreThan(18) } });
const user = await userRepository.findOne(1);

// Update
user.age = 29;
await userRepository.save(user);

// Delete
await userRepository.remove(user);
```

### Prisma (Modern ORM)

```javascript
// schema.prisma
model User {
  id        Int      @id @default(autoincrement())
  name      String
  email     String   @unique
  age       Int?
  createdAt DateTime @default(now())
  posts     Post[]
}

model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String?
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
}

// CRUD Operations
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();

// Create
const user = await prisma.user.create({
  data: {
    name: 'Alice',
    email: 'alice@example.com',
    age: 28
  }
});

// Read
const users = await prisma.user.findMany({
  where: { age: { gt: 18 } },
  include: { posts: true }
});

// Update
const updatedUser = await prisma.user.update({
  where: { email: 'alice@example.com' },
  data: { age: 29 }
});

// Delete
await prisma.user.delete({
  where: { email: 'alice@example.com' }
});
```

## ORM vs Raw SQL

```javascript
// Raw SQL
const query = `
  SELECT u.name, u.email, COUNT(p.id) as post_count
  FROM users u
  LEFT JOIN posts p ON u.id = p.author_id
  WHERE u.age > 18
  GROUP BY u.id
  HAVING COUNT(p.id) > 5
  ORDER BY post_count DESC
  LIMIT 10
`;
const results = await db.execute(query);

// ORM (Mongoose)
const users = await User.aggregate([
  { $lookup: { from: 'posts', localField: '_id', foreignField: 'author_id', as: 'posts' } },
  { $unwind: { path: '$posts', preserveNullAndEmptyArrays: true } },
  { $group: { _id: '$_id', name: { $first: '$name' }, postCount: { $sum: 1 } } },
  { $match: { postCount: { $gt: 5 } } },
  { $sort: { postCount: -1 } },
  { $limit: 10 }
]);

// ORM (Sequelize)
const users = await User.findAll({
  attributes: {
    include: [
      [sequelize.fn('COUNT', sequelize.col('posts.id')), 'postCount']
    ]
  },
  include: [{ model: Post, attributes: [] }],
  where: { age: { [Op.gt]: 18 } },
  group: ['User.id'],
  having: sequelize.literal('COUNT(posts.id) > 5'),
  order: [[sequelize.literal('postCount'), 'DESC']],
  limit: 10
});
```

## Benefits of ORM

```javascript
// 1. Database Independence
// Same code works with MySQL, PostgreSQL, SQLite, etc.
const sequelize = new Sequelize('sqlite::memory:'); // SQLite
const sequelize = new Sequelize('mysql://localhost/db'); // MySQL
const sequelize = new Sequelize('postgres://localhost/db'); // PostgreSQL

// 2. Type Safety (TypeScript)
interface User {
  id: number;
  name: string;
  email: string;
  age?: number;
}

// 3. Automatic Validation
const user = await User.create({
  name: 'Alice',
  email: 'invalid-email', // Validation error!
  age: -5 // Validation error!
});

// 4. Query Building
const users = await User.findAll({
  where: {
    age: { [Op.gte]: 18 },
    isActive: true
  },
  order: [['name', 'ASC']],
  limit: 10
});

// 5. Associations
const posts = await User.findAll({
  include: [{ model: Post, as: 'posts' }]
});
```

## Common Use Cases

- **Web Applications**: User management, content storage
- **APIs**: Data models for REST/GraphQL APIs
- **E-commerce**: Products, orders, user profiles
- **Social Networks**: Posts, comments, user connections
- **Enterprise Systems**: Complex data models and relationships
- **Microservices**: Service-specific data access layers

## Common Mistakes

1. **N+1 query problem**: Loading related data inefficiently
2. **Over-fetching**: Retrieving more data than needed
3. **Ignoring performance**: ORM queries can be slower than raw SQL
4. **Not using indexes**: ORM doesn't automatically create indexes
5. **Over-reliance on ORM**: Some operations are better with raw SQL

```javascript
// WRONG - N+1 query problem
const users = await User.findAll();
for (const user of users) {
  const posts = await Post.findAll({ where: { authorId: user.id } });
  // This executes N queries!
}

// CORRECT - Use eager loading
const users = await User.findAll({
  include: [{ model: Post, as: 'posts' }]
});
// This executes 1-2 queries!

// WRONG - Over-fetching
const users = await User.findAll();
// Returns all fields including password!

// CORRECT - Select specific fields
const users = await User.findAll({
  attributes: ['id', 'name', 'email']
});

// WRONG - Not using indexes
const users = await User.findAll({
  where: { email: 'alice@example.com' }
});
// Slow without index on email!

// CORRECT - Create indexes
User.addIndex('email', { unique: true });
const users = await User.findAll({
  where: { email: 'alice@example.com' }
});
```

## Quick Revision

- ORM maps objects to database tables
- Provides object-oriented way to interact with databases
- Popular JavaScript ORMs: Mongoose, Sequelize, TypeORM, Prisma
- Benefits: database independence, type safety, validation, query building
- Drawbacks: performance overhead, learning curve, complexity
- Use eager loading to avoid N+1 query problems
- Select specific fields to avoid over-fetching
- Create indexes for better performance
- Sometimes raw SQL is more efficient

## Related Topics

- [[What-is-MongoDB]]
- [[What-is-MySQL]]
- [[What-is-Mongoose]]
- [[Perform-CRUD]]
- [[Database-Design]]
