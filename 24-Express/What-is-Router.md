# What is Router in Express?

## Definition

A Router is an isolated instance of middleware and routes. It allows you to group related routes together, organize your application, and create modular, mountable route handlers. Routers help keep your code organized as your application grows.

```javascript
const express = require('express');
const router = express.Router();
```

## Basic Router Example

```javascript
const express = require('express');
const app = express();
const router = express.Router();

// Define routes on the router
router.get('/', (req, res) => {
  res.send('Users home page');
});

router.get('/profile', (req, res) => {
  res.send('User profile');
});

router.post('/', (req, res) => {
  res.send('Create new user');
});

// Mount the router on a path
app.use('/users', router);

// All routes will be prefixed with /users
// GET /users/
// GET /users/profile
// POST /users/
```

## Creating Separate Route Files

### routes/users.js

```javascript
const express = require('express');
const router = express.Router();

// Middleware specific to this router
router.use((req, res, next) => {
  console.log('Users route accessed');
  next();
});

router.get('/', (req, res) => {
  res.json({ users: ['Alice', 'Bob'] });
});

router.get('/:id', (req, res) => {
  res.json({ userId: req.params.id });
});

router.post('/', (req, res) => {
  res.json({ message: 'User created' });
});

router.put('/:id', (req, res) => {
  res.json({ message: `User ${req.params.id} updated` });
});

router.delete('/:id', (req, res) => {
  res.json({ message: `User ${req.params.id} deleted` });
});

module.exports = router;
```

### routes/products.js

```javascript
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  res.json({ products: ['Laptop', 'Phone'] });
});

router.get('/:id', (req, res) => {
  res.json({ productId: req.params.id });
});

module.exports = router;
```

### app.js

```javascript
const express = require('express');
const app = express();
const usersRouter = require('./routes/users');
const productsRouter = require('./routes/products');

app.use(express.json());

// Mount routers
app.use('/api/users', usersRouter);
app.use('/api/products', productsRouter);

app.listen(3000);
```

## Router Parameters

```javascript
const router = express.Router();

// Route parameters
router.get('/user/:id', (req, res) => {
  console.log(req.params.id); // Access route parameter
  res.send(`User ID: ${req.params.id}`);
});

// Multiple parameters
router.get('/user/:userId/post/:postId', (req, res) => {
  const { userId, postId } = req.params;
  res.send(`User ${userId}, Post ${postId}`);
});

// Router-level parameter middleware
router.param('id', (req, res, next, id) => {
  console.log(`Parameter ${id} processed`);
  req.userId = id; // Attach to request object
  next();
});

router.get('/user/:id', (req, res) => {
  res.send(`Processing user ${req.userId}`);
});
```

## Router Middleware

```javascript
const router = express.Router();

// Middleware that runs for all routes in this router
router.use((req, res, next) => {
  console.log('Router-level middleware');
  next();
});

// Middleware for specific routes
router.get('/admin', adminOnly, (req, res) => {
  res.send('Admin panel');
});

router.get('/profile', authenticate, (req, res) => {
  res.send('User profile');
});

function adminOnly(req, res, next) {
  if (req.user && req.user.isAdmin) {
    return next();
  }
  res.status(403).send('Admin only');
}

function authenticate(req, res, next) {
  // Authentication logic
  next();
}
```

## Nested Routers

```javascript
const express = require('express');
const app = express();

// Create parent router
const apiRouter = express.Router();

// Create child routers
const usersRouter = express.Router();
const postsRouter = express.Router();

// Define routes on child routers
usersRouter.get('/', (req, res) => res.send('All users'));
usersRouter.get('/:id', (req, res) => res.send(`User ${req.params.id}`));

postsRouter.get('/', (req, res) => res.send('All posts'));
postsRouter.get('/:id', (req, res) => res.send(`Post ${req.params.id}`));

// Mount child routers on parent
apiRouter.use('/users', usersRouter);
apiRouter.use('/posts', postsRouter);

// Mount parent router on app
app.use('/api', apiRouter);

// Routes:
// GET /api/users/
// GET /api/users/:id
// GET /api/posts/
// GET /api/posts/:id
```

## Router with Controllers

```javascript
// controllers/userController.js
exports.getAllUsers = (req, res) => {
  res.json({ users: [] });
};

exports.getUserById = (req, res) => {
  res.json({ user: req.params.id });
};

exports.createUser = (req, res) => {
  res.status(201).json({ message: 'User created' });
};

// routes/userRoutes.js
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');

router.get('/', userController.getAllUsers);
router.get('/:id', userController.getUserById);
router.post('/', userController.createUser);

module.exports = router;
```

## Common Use Cases

- **API Versioning**: Create separate routers for `/api/v1` and `/api/v2`
- **Resource Grouping**: Group all user-related routes under `/users`
- **Modular Architecture**: Split large applications into smaller, manageable files
- **Middleware Organization**: Apply middleware to specific route groups
- **Nested Resources**: Handle relationships like `/users/:id/posts`

## Common Mistakes

1. **Not exporting the router**: Always `module.exports = router`
2. **Forgetting to mount**: Define routes but forget `app.use('/path', router)`
3. **Confusing router and app**: Use `router` for route definitions, `app` for mounting
4. **Not using middleware correctly**: Router middleware only affects routes in that router
5. **Over-nesting**: Keep router hierarchy simple and readable

```javascript
// WRONG - Not mounting the router
const router = express.Router();
router.get('/', (req, res) => res.send('Hello'));
app.listen(3000); // Route won't work!

// CORRECT
const router = express.Router();
router.get('/', (req, res) => res.send('Hello'));
app.use('/', router); // Mount the router
app.listen(3000);
```

## Quick Revision

- Router is an isolated instance of middleware and routes
- Create with `express.Router()`
- Mount with `app.use('/path', router)`
- Supports route parameters with `:param` syntax
- Can have router-level middleware
- Supports nested routers for complex applications
- Helps organize code into modular, reusable components
- Router middleware only affects routes defined on that router

## Related Topics

- [[Use-Middleware]]
- [[Handle-Errors]]
- [[Express-Basics]]
- [[REST-API-Design]]
- [[Controllers]]
