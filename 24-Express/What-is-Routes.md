# What are Routes in Express?

**Routes** define how your application responds to client requests at specific URLs (endpoints) using HTTP methods (GET, POST, PUT, DELETE). Routes are fundamental to any web application.

## Definition

A route is a combination of a URL pattern and an HTTP method. When a client sends a request to your server, Express matches the URL and method to find the appropriate handler function.

```javascript
// Basic route structure
app.METHOD(PATH, HANDLER);

// Example
app.get('/', (req, res) => {
  res.send('Hello World!');
});
```

## Basic Route Syntax

```javascript
const express = require('express');
const app = express();

// GET request
app.get('/', (req, res) => {
  res.send('GET request to homepage');
});

// POST request
app.post('/submit', (req, res) => {
  res.send('POST request to submit');
});

// PUT request
app.put('/update', (req, res) => {
  res.send('PUT request to update');
});

// DELETE request
app.delete('/delete', (req, res) => {
  res.send('DELETE request to delete');
});

app.listen(3000);
```

## Route Parameters

```javascript
// Route parameters (required)
app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
  res.send(`User ID: ${userId}`);
});

// Multiple parameters
app.get('/users/:userId/posts/:postId', (req, res) => {
  const { userId, postId } = req.params;
  res.send(`User ${userId}, Post ${postId}`);
});

// Named parameters
app.get('/product/:id(\\d+)', (req, res) => {
  // Only matches numeric IDs
  res.send(`Product: ${req.params.id}`);
});
```

## Query Strings

```javascript
// Access query parameters
app.get('/search', (req, res) => {
  const { q, page, limit } = req.query;
  res.json({
    query: q,
    page: page || 1,
    limit: limit || 10,
  });
});

// Example: /search?q=nodejs&page=2&limit=5
// req.query = { q: 'nodejs', page: '2', limit: '5' }
```

## Router Module

### Creating a Router

```javascript
// routes/users.js
const express = require('express');
const router = express.Router();

// Define routes on router
router.get('/', (req, res) => {
  res.json({ users: [] });
});

router.get('/:id', (req, res) => {
  res.json({ user: { id: req.params.id } });
});

router.post('/', (req, res) => {
  res.status(201).json({ message: 'User created' });
});

module.exports = router;
```

### Using Router in Main App

```javascript
const express = require('express');
const app = express();
const usersRouter = require('./routes/users');
const postsRouter = require('./routes/posts');

// Mount routers
app.use('/users', usersRouter);
app.use('/posts', postsRouter);

app.listen(3000);
```

## Complete Router Example

### routes/index.js

```javascript
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  res.render('index', { title: 'Home' });
});

module.exports = router;
```

### routes/api.js

```javascript
const express = require('express');
const router = express.Router();

// All routes here are prefixed with /api
router.get('/status', (req, res) => {
  res.json({ status: 'ok' });
});

router.get('/users', (req, res) => {
  res.json({ users: [] });
});

module.exports = router;
```

### app.js

```javascript
const express = require('express');
const app = express();

const indexRouter = require('./routes/index');
const apiRouter = require('./routes/api');

app.use('/', indexRouter);
app.use('/api', apiRouter);

app.listen(3000);
```

## Route Groups

```javascript
const express = require('express');
const app = express();

// Public routes
const publicRouter = express.Router();
publicRouter.get('/login', (req, res) => res.send('Login'));
publicRouter.get('/register', (req, res) => res.send('Register'));

// Protected routes
const protectedRouter = express.Router();
protectedRouter.use(authMiddleware); // Apply middleware to all routes
protectedRouter.get('/dashboard', (req, res) => res.send('Dashboard'));
protectedRouter.get('/profile', (req, res) => res.send('Profile'));

// API routes
const apiRouter = express.Router();
apiRouter.use(express.json());
apiRouter.get('/users', (req, res) => res.json([]));
apiRouter.post('/users', (req, res) => res.status(201).json({}));

// Mount routes
app.use('/', publicRouter);
app.use('/app', protectedRouter);
app.use('/api', apiRouter);

function authMiddleware(req, res, next) {
  // Check authentication
  next();
}
```

## Route Order Matters

```javascript
// BAD - /users/new is matched first
app.get('/users', (req, res) => res.send('Users list'));
app.get('/users/new', (req, res) => res.send('New user'));

// GOOD - More specific routes first
app.get('/users/new', (req, res) => res.send('New user'));
app.get('/users', (req, res) => res.send('Users list'));

// Best practice - Use route parameters
app.get('/users/:action?', (req, res) => {
  if (req.params.action === 'new') {
    res.send('New user');
  } else {
    res.send('Users list');
  }
});
```

## Route Handlers

### Single Handler

```javascript
app.get('/user', (req, res) => {
  res.send('User');
});
```

### Multiple Handlers (Middleware Chain)

```javascript
// Array of handlers
app.get('/user',
  validateRequest,
  checkPermission,
  (req, res) => {
    res.send('User');
  }
);

function validateRequest(req, res, next) {
  // Validation logic
  next();
}

function checkPermission(req, res, next) {
  // Permission check
  next();
}
```

### Router-Level Handlers

```javascript
const router = express.Router();

// Apply middleware to all routes in this router
router.use(logRequest);

// Apply middleware to specific routes
router.get('/users', validateToken, (req, res) => {
  res.json([]);
});

function logRequest(req, res, next) {
  console.log(`${req.method} ${req.path}`);
  next();
}

function validateToken(req, res, next) {
  // Token validation
  next();
}
```

## Route Params Validation

```javascript
const express = require('express');
const app = express();

// Custom validation middleware
function validateIdParam(req, res, next) {
  const { id } = req.params;

  if (isNaN(id) || parseInt(id) < 1) {
    return res.status(400).json({ error: 'Invalid ID' });
  }

  req.params.id = parseInt(id);
  next();
}

// Apply to specific routes
app.get('/users/:id', validateIdParam, (req, res) => {
  res.json({ id: req.params.id });
});

// Or use regex in route pattern
app.get('/users/:id(\\d+)', (req, res) => {
  res.json({ id: req.params.id });
});
```

## Common Use Cases

- REST API endpoint definitions
- Page routing for web applications
- Versioned API endpoints
- Admin vs user routes
- Public vs protected routes
- Microservice endpoints

## Common Mistakes

### 1. Route Order Issues
```javascript
// Bad - /users/new never reached
app.get('/users', listUsers);
app.get('/users/new', newUserForm);

// Good - Specific routes first
app.get('/users/new', newUserForm);
app.get('/users', listUsers);
```

### 2. Forgetting to Export Router
```javascript
// Bad - Router not exported
const router = express.Router();
router.get('/', handler);

// Good - Export the router
module.exports = router;
```

### 3. Not Handling Route Parameters
```javascript
// Bad - No validation
app.get('/users/:id', (req, res) => {
  // req.params.id could be anything
});

// Good - Validate parameters
app.get('/users/:id', validateId, (req, res) => {
  // Safe to use req.params.id
});
```

### 4. Using Wrong HTTP Method
```javascript
// Bad - GET for creating resources
app.get('/users', createUser);

// Good - POST for creating
app.post('/users', createUser);
```

## Quick Revision

| Pattern | Example | Matches |
|---------|---------|---------|
| Static | `/users` | `/users` |
| Parameter | `/users/:id` | `/users/123` |
| Optional | `/users/:id?` | `/users` or `/users/123` |
| Wildcard | `/files/*` | `/files/a/b/c` |
| Regex | `/users/:id(\\d+)` | `/users/123` |

| Method | Purpose | Use Case |
|--------|---------|----------|
| GET | Retrieve | List/show resources |
| POST | Create | Create new resource |
| PUT | Update | Update entire resource |
| PATCH | Partial Update | Update parts of resource |
| DELETE | Delete | Remove resource |

## Related Topics

- [[What-is-Express]] - Express framework basics
- [[Handle-Methods]] - Handling HTTP methods in detail
- [[What-is-Middleware]] - Middleware in routes
- [[Create-App]] - Setting up Express apps
- [[What-is-HTTP]] - HTTP methods overview
- [[Create-Server]] - Building servers

---

**Key Takeaway:** Routes are the foundation of any Express application. Use routers to organize routes, place specific routes before general ones, and always validate route parameters.
