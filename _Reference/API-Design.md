# API Design Principles

## Definition

API (Application Programming Interface) design is the process of creating interfaces that allow different software components to communicate. Good API design focuses on consistency, simplicity, and developer experience (DX).

---

## RESTful API Design

### URL Structure

```javascript
// Good: resource-based URLs
// GET    /api/users          - List users
// GET    /api/users/:id      - Get user
// POST   /api/users          - Create user
// PUT    /api/users/:id      - Update user
// DELETE /api/users/:id      - Delete user

// Bad: action-based URLs
// GET /api/getUsers
// POST /api/createUser
// POST /api/deleteUser
```

### Status Codes

```javascript
// 2xx Success
200 OK                    // Successful GET, PUT, PATCH
201 Created               // Successful POST
204 No Content            // Successful DELETE

// 4xx Client Error
400 Bad Request           // Invalid input
401 Unauthorized          // Authentication required
403 Forbidden             // No permission
404 Not Found             // Resource doesn't exist
409 Conflict              // Duplicate resource
422 Unprocessable Entity  // Validation error
429 Too Many Requests     // Rate limit exceeded

// 5xx Server Error
500 Internal Server Error // Server crashed
502 Bad Gateway           // Upstream service error
503 Service Unavailable   // Server overloaded
```

### Request/Response Format

```javascript
// Good: consistent response structure
{
  "status": "success",
  "data": {
    "id": 1,
    "name": "Alice",
    "email": "alice@example.com"
  },
  "meta": {
    "timestamp": "2026-05-30T12:00:00Z"
  }
}

// Error response
{
  "status": "error",
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "details": [
      { "field": "email", "message": "Must be a valid email" }
    ]
  }
}
```

---

## API Versioning

### URL Versioning

```javascript
// Simple and clear
app.get("/api/v1/users", listUsers);
app.get("/api/v2/users", listUsersV2);
```

### Header Versioning

```javascript
// More RESTful
app.get("/api/users", (req, res) => {
  const version = req.headers["accept-version"];

  if (version === "2") {
    return listUsersV2(req, res);
  }

  return listUsers(req, res);
});
```

---

## Authentication & Authorization

### JWT Token Pattern

```javascript
// Client sends token in header
// Authorization: Bearer <token>

app.get("/api/profile", authenticate, (req, res) => {
  // req.user contains decoded token
  res.json(req.user);
});

// Middleware
function authenticate(req, res, next) {
  const token = req.headers.authorization?.split(" ")[1];

  if (!token) {
    return res.status(401).json({
      error: "Authentication required"
    });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    res.status(401).json({ error: "Invalid token" });
  }
}
```

### Role-Based Access

```javascript
function authorize(...roles) {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({
        error: "Insufficient permissions"
      });
    }
    next();
  };
}

// Usage
app.delete("/api/users/:id",
  authenticate,
  authorize("admin"),
  deleteUser
);
```

---

## Input Validation

### Schema Validation with Zod

```javascript
import { z } from "zod";

const CreateUserSchema = z.object({
  name: z.string().min(2).max(50),
  email: z.string().email(),
  age: z.number().int().min(18).max(120).optional()
});

app.post("/api/users", async (req, res) => {
  try {
    const data = CreateUserSchema.parse(req.body);
    const user = await createUser(data);
    res.status(201).json(user);
  } catch (err) {
    if (err instanceof z.ZodError) {
      return res.status(422).json({
        error: "Validation failed",
        details: err.errors
      });
    }
    res.status(500).json({ error: "Internal server error" });
  }
});
```

### Express Validator

```javascript
import { body, validationResult } from "express-validator";

const validateUser = [
  body("name").trim().isLength({ min: 2, max: 50 }),
  body("email").isEmail().normalizeEmail(),
  body("age").optional().isInt({ min: 18, max: 120 }),

  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(422).json({ errors: errors.array() });
    }
    next();
  }
];

app.post("/api/users", validateUser, createUser);
```

---

## Pagination

### Offset-Based Pagination

```javascript
app.get("/api/users", async (req, res) => {
  const { page = 1, limit = 10 } = req.query;
  const offset = (page - 1) * limit;

  const users = await db.users.findMany({
    skip: offset,
    take: parseInt(limit)
  });

  const total = await db.users.count();

  res.json({
    data: users,
    pagination: {
      page: parseInt(page),
      limit: parseInt(limit),
      total,
      pages: Math.ceil(total / limit)
    }
  });
});
```

### Cursor-Based Pagination

```javascript
app.get("/api/users", async (req, res) => {
  const { cursor, limit = 10 } = req.query;

  const users = await db.users.findMany({
    take: parseInt(limit) + 1, // Fetch one extra for next cursor
    skip: cursor ? 1 : 0,
    cursor: cursor ? { id: cursor } : undefined,
    orderBy: { id: "asc" }
  });

  const hasMore = users.length > parseInt(limit);
  const data = hasMore ? users.slice(0, -1) : users;
  const nextCursor = hasMore ? data[data.length - 1].id : null;

  res.json({
    data,
    pagination: {
      nextCursor,
      hasMore
    }
  });
});
```

---

## Rate Limiting

```javascript
import rateLimit from "express-rate-limit";

// Global rate limit
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per window
  standardHeaders: true,
  legacyHeaders: false,
  message: {
    error: "Too many requests",
    retryAfter: "15 minutes"
  }
});

app.use("/api/", limiter);

// Stricter limit for auth routes
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5, // 5 attempts per 15 minutes
  message: { error: "Too many login attempts" }
});

app.post("/api/auth/login", authLimiter, login);
```

---

## Error Handling

### Global Error Handler

```javascript
// Custom error class
class AppError extends Error {
  constructor(message, statusCode, code) {
    super(message);
    this.statusCode = statusCode;
    this.code = code;
  }
}

// Async error wrapper
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Routes with asyncHandler
app.get("/api/users/:id", asyncHandler(async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) throw new AppError("User not found", 404, "NOT_FOUND");
  res.json(user);
}));

// Global error handler
app.use((err, req, res, next) => {
  if (err instanceof AppError) {
    return res.status(err.statusCode).json({
      status: "error",
      error: {
        code: err.code,
        message: err.message
      }
    });
  }

  console.error(err);
  res.status(500).json({
    status: "error",
    error: {
      code: "INTERNAL_ERROR",
      message: "An unexpected error occurred"
    }
  });
});
```

---

## Common Use Cases

### API with Express

```javascript
import express from "express";
import { z } from "zod";

const app = express();
app.use(express.json());

const UserSchema = z.object({
  name: z.string().min(2),
  email: z.string().email()
});

const users = new Map();
let nextId = 1;

// List
app.get("/api/users", (req, res) => {
  res.json([...users.values()]);
});

// Get
app.get("/api/users/:id", (req, res) => {
  const user = users.get(parseInt(req.params.id));
  if (!user) return res.status(404).json({ error: "Not found" });
  res.json(user);
});

// Create
app.post("/api/users", (req, res) => {
  const data = UserSchema.parse(req.body);
  const user = { id: nextId++, ...data };
  users.set(user.id, user);
  res.status(201).json(user);
});

// Update
app.put("/api/users/:id", (req, res) => {
  const id = parseInt(req.params.id);
  if (!users.has(id)) return res.status(404).json({ error: "Not found" });
  const data = UserSchema.partial().parse(req.body);
  const user = { ...users.get(id), ...data };
  users.set(id, user);
  res.json(user);
});

// Delete
app.delete("/api/users/:id", (req, res) => {
  const id = parseInt(req.params.id);
  if (!users.delete(id)) return res.status(404).json({ error: "Not found" });
  res.status(204).end();
});

app.listen(3000);
```

---

## Common Mistakes

### Mistake 1: Inconsistent Error Responses

```javascript
// Wrong: different formats
res.status(400).json({ message: "Bad request" });
res.status(400).json({ error: "invalid" });
res.status(400).send("Bad request");

// Correct: consistent format
res.status(400).json({
  status: "error",
  error: {
    code: "VALIDATION_ERROR",
    message: "Bad request"
  }
});
```

### Mistake 2: Not Validating Input

```javascript
// Wrong: trusting client data
app.post("/api/users", (req, res) => {
  const user = db.createUser(req.body); // Dangerous!
});

// Correct: validate first
app.post("/api/users", validateInput, (req, res) => {
  const user = db.createUser(req.body); // Safe
});
```

### Mistake 3: Exposing Internal Errors

```javascript
// Wrong: leaking stack traces
app.use((err, req, res, next) => {
  res.status(500).json({
    error: err.message,
    stack: err.stack // Never expose this!
  });
});

// Correct: generic error message
app.use((err, req, res, next) => {
  console.error(err); // Log internally
  res.status(500).json({
    error: "Internal server error"
  });
});
```

### Mistake 4: No Pagination

```javascript
// Wrong: returning all records
app.get("/api/users", async (req, res) => {
  const users = await db.users.findMany(); // Could be millions!
  res.json(users);
});

// Correct: paginated response
app.get("/api/users", async (req, res) => {
  const { page = 1, limit = 10 } = req.query;
  // ... paginated query
});
```

---

## Quick Revision Summary

| Principle | Description |
|-----------|-------------|
| Resource-based URLs | `/api/users/:id` not `/api/getUser` |
| Proper status codes | 200, 201, 400, 404, 500 |
| Consistent responses | Same format for success and error |
| Input validation | Always validate client data |
| Pagination | Never return all records |
| Rate limiting | Protect against abuse |
| Versioning | `/api/v1/` or headers |
| Error handling | Never expose internal errors |

---

## Related Topics

- [[Promise]] - Async operations in API handlers
- [[Object]] - Request/response object patterns
- [[this]] - Context binding in Express middleware
- [[debugging]] - Debugging API issues
