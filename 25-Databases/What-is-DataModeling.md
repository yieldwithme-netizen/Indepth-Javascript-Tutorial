# What is Data Modeling

## Definition

**Data modeling** is the process of creating a data model that defines how data is stored, organized, and manipulated in a database. It involves defining tables, columns, relationships, constraints, and rules to ensure data integrity and efficient access.

## Why Data Modeling Matters

```javascript
// BAD: Poor data modeling - duplicate data
const users = [
  { id: 1, name: 'John', order: 'iPhone', orderDate: '2024-01-15' },
  { id: 1, name: 'John', order: 'MacBook', orderDate: '2024-02-20' },
  { id: 1, name: 'John', order: 'iPad', orderDate: '2024-03-10' }
];

// GOOD: Proper data modeling - normalized
const users = [
  { id: 1, name: 'John' }
];

const orders = [
  { id: 1, userId: 1, product: 'iPhone', date: '2024-01-15' },
  { id: 2, userId: 1, product: 'MacBook', date: '2024-02-20' },
  { id: 3, userId: 1, product: 'iPad', date: '2024-03-10' }
];
```

## Entity-Relationship Diagram

```javascript
// Define entities with Sequelize
const { Sequelize, Model, DataTypes } = require('sequelize');
const sequelize = new Sequelize('database', 'username', 'password', {
  dialect: 'mysql'
});

// User Entity
class User extends Model {}
User.init({
  id: { type: DataTypes.INTEGER, primaryKey: true, autoIncrement: true },
  name: { type: DataTypes.STRING, allowNull: false },
  email: { type: DataTypes.STRING, unique: true, allowNull: false }
}, { sequelize, modelName: 'User' });

// Product Entity
class Product extends Model {}
Product.init({
  id: { type: DataTypes.INTEGER, primaryKey: true, autoIncrement: true },
  name: { type: DataTypes.STRING, allowNull: false },
  price: { type: DataTypes.DECIMAL(10, 2), allowNull: false },
  stock: { type: DataTypes.INTEGER, defaultValue: 0 }
}, { sequelize, modelName: 'Product' });

// Order Entity (Relationship)
class Order extends Model {}
Order.init({
  id: { type: DataTypes.INTEGER, primaryKey: true, autoIncrement: true },
  quantity: { type: DataTypes.INTEGER, allowNull: false },
  total: { type: DataTypes.DECIMAL(10, 2), allowNull: false }
}, { sequelize, modelName: 'Order' });
```

## Normalization

```javascript
// 1NF: Atomic values, no repeating groups
// BAD
const user = {
  name: 'John',
  hobbies: 'reading, gaming, coding'  // Multiple values in one field
};

// GOOD
const user = {
  name: 'John',
  hobbies: ['reading', 'gaming', 'coding']  // Array or separate table
};

// 2NF: No partial dependencies
// BAD (composite primary key with partial dependency)
const orderItem = {
  orderId: 1,
  productId: 5,
  productName: 'iPhone',  // Depends only on productId
  quantity: 2
};

// GOOD
const orderItem = {
  orderId: 1,
  productId: 5,
  quantity: 2
};

// 3NF: No transitive dependencies
// BAD
const user = {
  id: 1,
  name: 'John',
  department: 'Engineering',
  departmentHead: 'Jane'  // Depends on department, not user
};

// GOOD
const user = {
  id: 1,
  name: 'John',
  departmentId: 1
};

const department = {
  id: 1,
  name: 'Engineering',
  headId: 2
};
```

## Common Relationships

```javascript
// One-to-One
User.hasOne(Profile);
Profile.belongsTo(User);

// One-to-Many
User.hasMany(Order);
Order.belongsTo(User);

// Many-to-Many
User.belongsToMany(Role, { through: 'UserRoles' });
Role.belongsToMany(User, { through: 'UserRoles' });
```

## Schema Design Best Practices

```javascript
// Use appropriate data types
User.init({
  id: { type: DataTypes.INTEGER, primaryKey: true },
  name: { type: DataTypes.STRING(100) },  // Limit length
  email: { type: DataTypes.STRING },      // Email validation
  age: { type: DataTypes.INTEGER },       // Numeric
  bio: { type: DataTypes.TEXT },          // Long text
  isActive: { type: DataTypes.BOOLEAN },  // Boolean
  salary: { type: DataTypes.DECIMAL(10, 2) },  // Currency
  createdAt: { type: DataTypes.DATE }     // Timestamp
}, { sequelize, modelName: 'User' });

// Add constraints
User.init({
  email: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true,
    validate: { isEmail: true }
  },
  age: {
    type: DataTypes.INTEGER,
    validate: { min: 0, max: 150 }
  }
}, { sequelize, modelName: 'User' });
```

## Common Mistakes

1. **Denormalizing too early**: Start normalized, denormalize only when needed
2. **Ignoring relationships**: Define foreign keys and associations
3. **Using wrong data types**: Impact on storage and query performance
4. **Not planning for growth**: Consider future data volume
5. **Skipping validation**: Add constraints at database level

## Related Topics

- [[SQL-vs-NoSQL]]
- [[What-is-Indexing]]
- [[Use-Sequelize]]

## Quick Revision

- Data modeling defines how data is structured and related
- Follow normalization rules (1NF, 2NF, 3NF)
- Use appropriate data types and constraints
- Define relationships (1:1, 1:N, N:N)
- Plan for scalability and future requirements
