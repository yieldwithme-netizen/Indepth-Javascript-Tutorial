# Schema Design in JavaScript

## Definition

Schema design refers to the structure and organization of data in applications. In JavaScript, schemas define the shape, types, and constraints of data objects, commonly used with databases, validation libraries, and API design.

## Schema Types

### 1. JSON Schema

```javascript
const userSchema = {
  type: "object",
  properties: {
    id: { type: "integer" },
    name: { type: "string", minLength: 1, maxLength: 100 },
    email: { type: "string", format: "email" },
    age: { type: "integer", minimum: 0, maximum: 150 },
    isActive: { type: "boolean", default: true },
    roles: {
      type: "array",
      items: { type: "string" },
      minItems: 1
    }
  },
  required: ["id", "name", "email"],
  additionalProperties: false
};
```

### 2. Mongoose Schema (MongoDB)

```javascript
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    trim: true,
    minlength: 2,
    maxlength: 50
  },
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    match: [/^\w+@\w+\.\w+$/, "Invalid email"]
  },
  password: {
    type: String,
    required: true,
    minlength: 8
  },
  role: {
    type: String,
    enum: ["user", "admin", "moderator"],
    default: "user"
  },
  profile: {
    bio: String,
    avatar: String,
    website: String
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
}, {
  timestamps: true,
  toJSON: { virtuals: true }
});

// Virtual field
userSchema.virtual("profileSummary").get(function() {
  return `${this.name} (${this.email})`;
});

// Index for search
userSchema.index({ email: 1 });

const User = mongoose.model("User", userSchema);
```

### 3. Yup Validation Schema

```javascript
const yup = require("yup");

const userSchema = yup.object({
  name: yup.string().required().min(2).max(50),
  email: yup.string().email().required(),
  age: yup.number().positive().integer().max(150),
  password: yup.string().min(8).matches(
    /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/,
    "Password must contain uppercase, lowercase, and number"
  ),
  confirmPassword: yup.string().oneOf(
    [yup.ref("password")],
    "Passwords must match"
  ),
  website: yup.string().url().nullable(),
  roles: yup.array().of(yup.string()).min(1)
});

// Async validation
async function validateUser(userData) {
  try {
    await userSchema.validate(userData, { abortEarly: false });
    return { valid: true };
  } catch (err) {
    return { valid: false, errors: err.errors };
  }
}
```

### 4. Joi Schema

```javascript
const Joi = require("joi");

const userSchema = Joi.object({
  name: Joi.string().min(2).max(50).required(),
  email: Joi.string().email().required(),
  age: Joi.number().integer().min(0).max(150),
  password: Joi.string().min(8).pattern(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/),
  roles: Joi.array().items(Joi.string().valid("user", "admin")).min(1)
});

const { error, value } = userSchema.validate(userData);
```

### 5. TypeScript Interface as Schema

```typescript
// TypeScript provides compile-time schema validation
interface User {
  id: number;
  name: string;
  email: string;
  age?: number;
  roles: Role[];
  createdAt: Date;
}

enum Role {
  User = "user",
  Admin = "admin",
  Moderator = "moderator"
}

// Runtime validation with zod
import { z } from "zod";

const UserSchema = z.object({
  id: z.number(),
  name: z.string().min(1).max(100),
  email: z.string().email(),
  age: z.number().optional(),
  roles: z.array(z.nativeEnum(Role)).min(1),
  createdAt: z.date()
});

type User = z.infer<typeof UserSchema>;
```

## Code Examples

### Database Schema Design

```javascript
// Relational-style with MongoDB
const orderSchema = new mongoose.Schema({
  customer: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
    required: true
  },
  items: [{
    product: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "Product"
    },
    quantity: { type: Number, min: 1 },
    price: { type: Number, min: 0 }
  }],
  total: { type: Number, min: 0 },
  status: {
    type: String,
    enum: ["pending", "processing", "shipped", "delivered"],
    default: "pending"
  }
}, { timestamps: true });
```

### Schema Validation Middleware

```javascript
// Express validation middleware
function validate(schema) {
  return (req, res, next) => {
    const { error, value } = schema.validate(req.body, {
      abortEarly: false,
      stripUnknown: true
    });

    if (error) {
      return res.status(400).json({
        errors: error.details.map(d => d.message)
      });
    }

    req.body = value;
    next();
  };
}

// Usage
app.post("/users", validate(userSchema), createUser);
```

## Common Use Cases

- Database modeling and relationships
- API request/response validation
- Form validation on client-side
- Configuration file validation
- Data transformation and sanitization

## Common Mistakes

1. **Not validating data**: Always validate before processing
2. **Overly complex schemas**: Keep schemas simple and focused
3. **Missing indexes**: Can cause performance issues
4. **Not using defaults**: Reduces boilerplate code

## Related Topics

- [[Validation]] - Data validation techniques
- [[TypeScript-Types]] - Static type checking
- [[MongoDB]] - NoSQL database schemas
- [[PostgreSQL]] - SQL database schemas
- [[API-Design]] - RESTful API schema patterns
- [[Form-Validation]] - Client-side validation

## Quick Revision Summary

| Library | Use Case | Features |
|---------|----------|----------|
| JSON Schema | Standard validation | Universal, declarative |
| Mongoose | MongoDB ODM | Built-in validation, middleware |
| Yup | Form validation | Promise-based, chainable |
| Joi | Server validation | Rich API, extensible |
| Zod | TypeScript-first | Static typing, inference |
