# How to Handle HTTP Methods in Express

Handling HTTP methods correctly is essential for building RESTful APIs and web applications. Express provides clean methods for each HTTP verb.

## Definition

HTTP methods (verbs) indicate the desired action to be performed on a resource. Express provides dedicated methods for each: `get()`, `post()`, `put()`, `patch()`, `delete()`, `options()`, and `all()`.

```javascript
// Express method handlers
app.get(path, handler);     // Read
app.post(path, handler);    // Create
app.put(path, handler);     // Update (full)
app.patch(path, handler);   // Update (partial)
app.delete(path, handler);  // Delete
app.all(path, handler);     // All methods
```

## Basic Method Handlers

```javascript
const express = require('express');
const app = express();

app.use(express.json());

// GET - Read resources
app.get('/api/users', (req, res) => {
  res.json({ users: [] });
});

// POST - Create resources
app.post('/api/users', (req, res) => {
  const { name, email } = req.body;
  res.status(201).json({ id: 1, name, email });
});

// PUT - Update entire resource
app.put('/api/users/:id', (req, res) => {
  const { id } = req.params;
  const { name, email } = req.body;
  res.json({ id, name, email });
});

// PATCH - Partial update
app.patch('/api/users/:id', (req, res) => {
  const { id } = req.params;
  const updates = req.body;
  res.json({ id, ...updates });
});

// DELETE - Remove resource
app.delete('/api/users/:id', (req, res) => {
  res.status(204).send();
});

app.listen(3000);
```

## Complete CRUD Example

```javascript
const express = require('express');
const app = express();
app.use(express.json());

let users = [
  { id: 1, name: 'Alice', email: 'alice@example.com' },
  { id: 2, name: 'Bob', email: 'bob@example.com' },
];

// GET - List all users
app.get('/api/users', (req, res) => {
  res.json(users);
});

// GET - Get user by ID
app.get('/api/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));

  if (!user) {
    return res.status(404).json({ error: 'User not found' });
  }

  res.json(user);
});

// POST - Create user
app.post('/api/users', (req, res) => {
  const { name, email } = req.body;

  // Validation
  if (!name || !email) {
    return res.status(400).json({ error: 'Name and email required' });
  }

  // Check duplicate
  if (users.find(u => u.email === email)) {
    return res.status(409).json({ error: 'Email already exists' });
  }

  const newUser = {
    id: users.length > 0 ? Math.max(...users.map(u => u.id)) + 1 : 1,
    name,
    email,
  };

  users.push(newUser);
  res.status(201).json(newUser);
});

// PUT - Replace user
app.put('/api/users/:id', (req, res) => {
  const { id } = req.params;
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({ error: 'Name and email required' });
  }

  const index = users.findIndex(u => u.id === parseInt(id));

  if (index === -1) {
    return res.status(404).json({ error: 'User not found' });
  }

  users[index] = { id: parseInt(id), name, email };
  res.json(users[index]);
});

// PATCH - Partial update
app.patch('/api/users/:id', (req, res) => {
  const { id } = req.params;
  const index = users.findIndex(u => u.id === parseInt(id));

  if (index === -1) {
    return res.status(404).json({ error: 'User not found' });
  }

  // Only update provided fields
  if (req.body.name) users[index].name = req.body.name;
  if (req.body.email) users[index].email = req.body.email;

  res.json(users[index]);
});

// DELETE - Remove user
app.delete('/api/users/:id', (req, res) => {
  const index = users.findIndex(u => u.id === parseInt(req.params.id));

  if (index === -1) {
    return res.status(404).json({ error: 'User not found' });
  }

  users.splice(index, 1);
  res.status(204).send();
});

app.listen(3000);
```

## Handling Multiple Methods

```javascript
const express = require('express');
const app = express();

// Handle multiple methods with app.all()
app.all('/api/resource', (req, res) => {
  res.json({ method: req.method, message: 'Handled by app.all()' });
});

// Route with multiple methods
const userMethods = {
  GET: (req, res) => res.json({ action: 'get' }),
  POST: (req, res) => res.json({ action: 'post' }),
  PUT: (req, res) => res.json({ action: 'put' }),
  DELETE: (req, res) => res.json({ action: 'delete' }),
};

app.route('/api/items')
  .get(userMethods.GET)
  .post(userMethods.POST);

app.route('/api/items/:id')
  .put(userMethods.PUT)
  .delete(userMethods.DELETE);

app.listen(3000);
```

## Request Body Parsing

```javascript
const express = require('express');
const app = express();

// Parse JSON bodies
app.use(express.json());

// Parse URL-encoded bodies
app.use(express.urlencoded({ extended: true }));

// Parse raw bodies
app.use(express.raw({ type: 'application/octet-stream' }));

// POST with JSON body
app.post('/api/data', (req, res) => {
  console.log('Body:', req.body);
  res.json({ received: req.body });
});

// POST with form data
app.post('/api/form', (req, res) => {
  console.log('Form data:', req.body);
  res.json({ received: req.body });
});

app.listen(3000);
```

## PUT vs PATCH

```javascript
const express = require('express');
const app = express();
app.use(express.json());

let user = { id: 1, name: 'Alice', email: 'alice@example.com' };

// PUT - Replace entire resource
// Client must send ALL fields
app.put('/api/user', (req, res) => {
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({ error: 'All fields required for PUT' });
  }

  user = { id: user.id, name, email };
  res.json(user);
});

// PATCH - Partial update
// Client only sends fields to update
app.patch('/api/user', (req, res) => {
  // Merge updates with existing data
  user = { ...user, ...req.body };
  res.json(user);
});

app.listen(3000);

// PUT: { "name": "Alice" } → 400 (missing email)
// PATCH: { "name": "Alice" } → 200 (email unchanged)
```

## OPTIONS and CORS

```javascript
const express = require('express');
const app = express();

// CORS middleware
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Methods', 'GET, POST, PUT, PATCH, DELETE, OPTIONS');
  res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization');

  // Handle preflight
  if (req.method === 'OPTIONS') {
    return res.sendStatus(204);
  }

  next();
});

app.get('/api/data', (req, res) => {
  res.json({ data: 'value' });
});

app.listen(3000);
```

## Method-Related Headers

```javascript
const express = require('express');
const app = express();

app.get('/api/data', (req, res) => {
  // Request headers
  console.log('Accept:', req.get('Accept'));
  console.log('Authorization:', req.get('Authorization'));
  console.log('Content-Type:', req.get('Content-Type'));

  // Set response headers
  res.set('X-Custom-Header', 'value');
  res.set('Cache-Control', 'no-cache');

  res.json({ data: 'value' });
});

app.listen(3000);
```

## Status Codes by Method

```javascript
const express = require('express');
const app = express();
app.use(express.json());

// GET - 200 OK
app.get('/api/resource', (req, res) => {
  res.status(200).json({ data: [] });
});

// POST - 201 Created
app.post('/api/resource', (req, res) => {
  res.status(201).json({ id: 1, ...req.body });
});

// PUT - 200 OK or 204 No Content
app.put('/api/resource/:id', (req, res) => {
  res.status(200).json({ updated: true });
});

// PATCH - 200 OK
app.patch('/api/resource/:id', (req, res) => {
  res.status(200).json({ patched: true });
});

// DELETE - 204 No Content
app.delete('/api/resource/:id', (req, res) => {
  res.status(204).send();
});

// Error responses
app.get('/api/missing', (req, res) => {
  res.status(404).json({ error: 'Not found' });
});

app.post('/api/invalid', (req, res) => {
  res.status(400).json({ error: 'Bad request' });
});

app.listen(3000);
```

## Common Use Cases

- Building RESTful APIs
- CRUD applications
- Form handling (GET for forms, POST for submissions)
- File uploads (POST/PUT)
- Webhook receivers
- Admin panels (different methods for different actions)

## Common Mistakes

### 1. Using Wrong Methods
```javascript
// Bad - Using GET for mutations
app.get('/api/users/delete/:id', deleteUser);

// Good - Use DELETE method
app.delete('/api/users/:id', deleteUser);
```

### 2. Not Parsing Request Body
```javascript
// Bad - Body is undefined
app.post('/api/data', (req, res) => {
  console.log(req.body); // undefined
});

// Good - Add body parser middleware
app.use(express.json());
app.post('/api/data', (req, res) => {
  console.log(req.body); // parsed object
});
```

### 3. Wrong Status Codes
```javascript
// Bad - Always 200
app.post('/api/users', (req, res) => {
  res.json(newUser); // Should be 201
});

// Good - Correct status codes
app.post('/api/users', (req, res) => {
  res.status(201).json(newUser);
});
```

### 4. Not Handling OPTIONS for CORS
```javascript
// Bad - CORS fails for preflight requests
app.get('/api/data', handler);

// Good - Handle OPTIONS
app.options('/api/data', corsHandler);
app.get('/api/data', handler);
```

## Quick Revision

| Method | Purpose | Status | Body |
|--------|---------|--------|------|
| GET | Read | 200 | No |
| POST | Create | 201 | Yes |
| PUT | Replace | 200/204 | Yes |
| PATCH | Update | 200 | Yes |
| DELETE | Remove | 204 | No |

| Method | Idempotent | Safe | Cacheable |
|--------|------------|------|-----------|
| GET | Yes | Yes | Yes |
| POST | No | No | No |
| PUT | Yes | No | No |
| PATCH | No | No | No |
| DELETE | Yes | No | No |

## Related Topics

- [[What-is-Routes]] - Route definitions
- [[What-is-Middleware]] - Request processing
- [[What-is-Express]] - Express basics
- [[Create-App]] - Setting up Express
- [[What-is-HTTP]] - HTTP methods overview
- [[Create-Server]] - Server creation

---

**Key Takeaway:** Use the correct HTTP methods for their intended purpose. GET for reading, POST for creating, PUT/PATCH for updating, DELETE for removing. Always return appropriate status codes.
