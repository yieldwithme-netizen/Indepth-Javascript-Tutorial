# Use Sequelize

## Definition

**Sequelize** is a promise-based Node.js ORM (Object-Relational Mapping) for SQL databases like PostgreSQL, MySQL, SQLite, and Microsoft SQL Server. It provides an abstraction layer to interact with databases using JavaScript instead of raw SQL queries.

## Installation

```bash
npm install sequelize
# For MySQL
npm install mysql2
# For PostgreSQL
npm install pg pg-hstore
```

## Basic Setup

```javascript
const { Sequelize, Model, DataTypes } = require('sequelize');

// Initialize Sequelize
const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'mysql', // or 'postgres', 'sqlite', 'mssql'
  logging: false
});

// Test connection
async function testConnection() {
  try {
    await sequelize.authenticate();
    console.log('Connection established successfully.');
  } catch (error) {
    console.error('Unable to connect:', error);
  }
}
```

## Defining Models

```javascript
// User model
class User extends Model {}

User.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  name: {
    type: DataTypes.STRING,
    allowNull: false,
    validate: {
      notEmpty: true,
      len: [2, 50]
    }
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
  sequelize,
  modelName: 'User',
  tableName: 'users',
  timestamps: true
});

// Post model
class Post extends Model {}

Post.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  title: {
    type: DataTypes.STRING,
    allowNull: false
  },
  content: {
    type: DataTypes.TEXT
  },
  published: {
    type: DataTypes.BOOLEAN,
    defaultValue: false
  }
}, {
  sequelize,
  modelName: 'Post',
  tableName: 'posts'
});
```

## Associations

```javascript
// One-to-Many: User has many Posts
User.hasMany(Post, { foreignKey: 'userId', as: 'posts' });
Post.belongsTo(User, { foreignKey: 'userId', as: 'author' });

// Many-to-Many
class Tag extends Model {}
Tag.init({
  name: DataTypes.STRING
}, { sequelize, modelName: 'Tag' });

Post.belongsToMany(Tag, { through: 'PostTags' });
Tag.belongsToMany(Post, { through: 'PostTags' });
```

## CRUD Operations

```javascript
// Create
const user = await User.create({
  name: 'John Doe',
  email: 'john@example.com',
  age: 30
});

// Read
const foundUser = await User.findByPk(1);
const users = await User.findAll({
  where: { age: { [Op.gte]: 18 } },
  include: ['posts'],
  order: [['name', 'ASC']],
  limit: 10
});

// Update
await user.update({ name: 'Jane Doe' });

// Delete
await user.destroy();
```

## Querying

```javascript
const { Op } = require('sequelize');

// Complex queries
const results = await User.findAll({
  where: {
    [Op.and]: [
      { age: { [Op.between]: [18, 65] } },
      { email: { [Op.like]: '%@gmail.com' } }
    ]
  },
  attributes: ['id', 'name', 'email'],
  include: [{
    model: Post,
    as: 'posts',
    where: { published: true },
    required: false
  }],
  group: ['User.id'],
  having: sequelize.literal('COUNT(posts.id) > 0')
});
```

## Syncing Models

```javascript
// Sync all models
await sequelize.sync({ alter: true }); // or { force: true }

// Sync specific model
await User.sync();
```

## Common Mistakes

1. Not using transactions for related operations
2. Forgetting to handle associations in queries
3. Overusing raw SQL instead of Sequelize methods
4. Not validating data before database operations
5. Ignoring N+1 query problems

## Related Topics

- [[SQL-vs-NoSQL]]
- [[What-is-DataModeling]]
- [[What-is-Indexing]]

## Quick Revision

- Sequelize is an ORM for SQL databases in Node.js
- Define models using `Model.init()` or class syntax
- Use `belongsTo`, `hasMany`, `belongsToMany` for associations
- Leverage `findAll`, `findByPk`, `create`, `update`, `destroy` for CRUD
- Use transactions for data integrity
