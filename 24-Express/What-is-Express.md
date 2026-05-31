# What is Express.js?

## Definition

Express.js is a **web framework for Node.js** that simplifies HTTP server creation.

## Basic App

```javascript
const express = require('express');
const app = express();

// Routes
app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.get('/users', (req, res) => {
    res.json([{ name: 'John' }, { name: 'Jane' }]);
});

// Start server
app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

## Routes

```javascript
// GET
app.get('/users', (req, res) => { });

// POST
app.post('/users', (req, res) => { });

// PUT
app.put('/users/:id', (req, res) => { });

// DELETE
app.delete('/users/:id', (req, res) => { });
```

## Middleware

```javascript
// Built-in
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Custom
const logger = (req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next();
};
app.use(logger);

// Static files
app.use(express.static('public'));
```

## Quick Revision

- Express = Node.js web framework
- Routes: get, post, put, delete
- Middleware: functions that run before route
- `req` = request, `res` = response
- Use for: APIs, web apps

---

## Related Topics

- [[What-is-Express]] - Express overview
- [[Create-App]] - Creating Express app
- [[What-is-Routes]] - Routes
- [[Handle-Methods]] - HTTP methods
- [[What-is-Middleware]] - Middleware
- [[Use-Middleware]] - Using middleware
