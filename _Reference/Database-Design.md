# Database Design

Database design is the process of creating a well-structured database that efficiently stores, retrieves, and manages data. It's crucial for building scalable and maintainable applications.

## What is Database Design?

Database design involves:

- **Schema design** - Defining tables, columns, and relationships
- **Normalization** - Organizing data to reduce redundancy
- **Indexing** - Optimizing query performance
- **Constraints** - Ensuring data integrity
- **Security** - Protecting sensitive data

## Relational Database Design

### Normalization

**First Normal Form (1NF):**
- Each cell contains single values
- Each record is unique

```sql
-- Bad: Multiple values in one column
CREATE TABLE users (
  id INT,
  hobbies VARCHAR(255) -- "reading,gaming,cooking"
);

-- Good: Separate table for hobbies
CREATE TABLE users (
  id INT PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE user_hobbies (
  user_id INT,
  hobby VARCHAR(100),
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

**Second Normal Form (2NF):**
- Must be in 1NF
- No partial dependencies

```sql
-- Bad: Partial dependency
CREATE TABLE orders (
  order_id INT PRIMARY KEY,
  product_id INT,
  product_name VARCHAR(100), -- Depends only on product_id
  customer_name VARCHAR(100) -- Depends only on customer_id
);

-- Good: Separate tables
CREATE TABLE products (
  id INT PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE customers (
  id INT PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE orders (
  id INT PRIMARY KEY,
  product_id INT,
  customer_id INT,
  FOREIGN KEY (product_id) REFERENCES products(id),
  FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

### Relationships

```sql
-- One-to-Many
CREATE TABLE customers (
  id INT PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE orders (
  id INT PRIMARY KEY,
  customer_id INT,
  FOREIGN KEY (customer_id) REFERENCES customers(id)
);

-- Many-to-Many
CREATE TABLE students (
  id INT PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE courses (
  id INT PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE enrollments (
  student_id INT,
  course_id INT,
  PRIMARY KEY (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES students(id),
  FOREIGN KEY (course_id) REFERENCES courses(id)
);
```

## NoSQL Database Design

### MongoDB Document Structure

```javascript
// Single document (embedded)
const userSchema = {
  _id: ObjectId,
  name: String,
  email: String,
  address: {
    street: String,
    city: String,
    zip: String
  },
  orders: [
    {
      productId: ObjectId,
      quantity: Number,
      date: Date
    }
  ]
};

// Reference pattern (normalized)
const userSchema = {
  _id: ObjectId,
  name: String,
  email: String,
  addressId: ObjectId // References address collection
};
```

## Indexing

```sql
-- Create index for faster queries
CREATE INDEX idx_users_email ON users(email);

-- Composite index
CREATE INDEX idx_orders_customer_date 
ON orders(customer_id, order_date);

-- Unique index
CREATE UNIQUE INDEX idx_users_email 
ON users(email);
```

```javascript
// MongoDB indexing
db.users.createIndex({ email: 1 }, { unique: true });
db.orders.createIndex({ customer_id: 1, order_date: -1 });
```

## Common Patterns

### Repository Pattern

```javascript
class UserRepository {
  constructor(db) {
    this.db = db;
  }

  async findById(id) {
    return this.db.collection('users').findOne({ _id: id });
  }

  async findByEmail(email) {
    return this.db.collection('users').findOne({ email });
  }

  async create(userData) {
    return this.db.collection('users').insertOne(userData);
  }

  async update(id, updates) {
    return this.db.collection('users').updateOne(
      { _id: id },
      { $set: updates }
    );
  }

  async delete(id) {
    return this.db.collection('users').deleteOne({ _id: id });
  }
}
```

## Common Use Cases

- E-commerce platforms
- Social media applications
- Content management systems
- Financial systems
- Healthcare applications

## Common Mistakes

1. **Poor normalization** - Too much or too little
2. **Missing indexes** - Slow query performance
3. **No constraints** - Data integrity issues
4. **Over-normalization** - Complex joins
5. **Ignoring security** - SQL injection vulnerabilities

## Related Topics

- [[SQL-Basics]]
- [[MongoDB]]
- [[ORM-Libraries]]
- [[Migrations]]
- [[CRUD-Operations]]

## Quick Revision

| Concept | Purpose |
|---------|---------|
| Normalization | Reduce data redundancy |
| Indexing | Improve query performance |
| Constraints | Ensure data integrity |
| Relationships | Connect related data |
| Schema | Define data structure |

Good database design is essential for building efficient, scalable, and maintainable applications.