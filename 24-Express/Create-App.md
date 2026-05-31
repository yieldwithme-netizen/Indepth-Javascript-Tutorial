# How to Create an Express App

**Express.js** is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications. It simplifies the process of building web servers.

## Definition

Express is a lightweight framework built on top of Node.js's `http` module. It provides a cleaner API for routing, middleware, and handling HTTP requests and responses.

```bash
# Initialize project and install Express
npm init -y
npm install express
```

## Basic Express Application

```javascript
const express = require('express');
const app = express();
const port = 3000;

// Define a route
app.get('/', (req, res) => {
  res.send('Hello, World!');
});

// Start server
app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

## Project Structure

```
my-express-app/
├── node_modules/
├── routes/
│   ├── index.js
│   ├── users.js
│   └── api.js
├── middleware/
│   ├── auth.js
│   └── logger.js
├── controllers/
│   ├── userController.js
│   └── postController.js
├── public/
│   ├── css/
│   └── js/
├── views/
│   └── index.html
├── app.js
├── package.json
└── .env
```

## Express Application File (app.js)

```javascript
const express = require('express');
const path = require('path');
require('dotenv').config();

// Create Express app
const app = express();

// Import routes
const indexRouter = require('./routes/index');
const usersRouter = require('./routes/users');
const apiRouter = require('./routes/api');

// Middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.static(path.join(__dirname, 'public')));

// Routes
app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/api', apiRouter);

// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});

// Start server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

module.exports = app;
```

## Setting Up Routes

### routes/index.js

```javascript
const express = require('express');
const router = express.Router();

// Home route
router.get('/', (req, res) => {
  res.render('index', { title: 'Home' });
});

// About route
router.get('/about', (req, res) => {
  res.send('About Page');
});

module.exports = router;
```

### routes/users.js

```javascript
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');

// Get all users
router.get('/', userController.getAllUsers);

// Get user by ID
router.get('/:id', userController.getUserById);

// Create new user
router.post('/', userController.createUser);

// Update user
router.put('/:id', userController.updateUser);

// Delete user
router.delete('/:id', userController.deleteUser);

module.exports = router;
```

## Controllers

### controllers/userController.js

```javascript
// In-memory data (use database in production)
let users = [
  { id: 1, name: 'Alice', email: 'alice@example.com' },
  { id: 2, name: 'Bob', email: 'bob@example.com' },
];

const userController = {
  // Get all users
  getAllUsers: (req, res) => {
    res.json(users);
  },

  // Get user by ID
  getUserById: (req, res) => {
    const user = users.find(u => u.id === parseInt(req.params.id));

    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }

    res.json(user);
  },

  // Create new user
  createUser: (req, res) => {
    const { name, email } = req.body;

    if (!name || !email) {
      return res.status(400).json({ error: 'Name and email required' });
    }

    const newUser = {
      id: users.length + 1,
      name,
      email,
    };

    users.push(newUser);
    res.status(201).json(newUser);
  },

  // Update user
  updateUser: (req, res) => {
    const user = users.find(u => u.id === parseInt(req.params.id));

    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }

    const { name, email } = req.body;
    if (name) user.name = name;
    if (email) user.email = email;

    res.json(user);
  },

  // Delete user
  deleteUser: (req, res) => {
    const index = users.findIndex(u => u.id === parseInt(req.params.id));

    if (index === -1) {
      return res.status(404).json({ error: 'User not found' });
    }

    users.splice(index, 1);
    res.status(204).send();
  },
};

module.exports = userController;
```

## Middleware Setup

### middleware/logger.js

```javascript
const logger = (req, res, next) => {
  const start = Date.now();

  res.on('finish', () => {
    const duration = Date.now() - start;
    console.log(`${req.method} ${req.originalUrl} ${res.statusCode} ${duration}ms`);
  });

  next();
};

module.exports = logger;
```

### middleware/auth.js

```javascript
const auth = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];

  if (!token) {
    return res.status(401).json({ error: 'Authentication required' });
  }

  try {
    // Verify token (use JWT in production)
    const decoded = { id: 1, role: 'user' }; // jwt.verify(token, secret)
    req.user = decoded;
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
};

module.exports = auth;
```

## Environment Configuration

### .env

```
PORT=3000
NODE_ENV=development
DB_HOST=localhost
DB_PORT=27017
DB_NAME=myapp
JWT_SECRET=your-secret-key
```

### Loading Environment Variables

```javascript
require('dotenv').config();

const port = process.env.PORT || 3000;
const dbHost = process.env.DB_HOST;
```

## Static Files

```javascript
const path = require('path');

// Serve static files from 'public' directory
app.use(express.static(path.join(__dirname, 'public')));

// Serve with a route prefix
app.use('/static', express.static(path.join(__dirname, 'public')));

// Serve with cache control
app.use(express.static('public', {
  maxAge: '1d',
  etag: true,
}));
```

## Template Engines

```javascript
// Set view engine
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, 'views'));

// In route
app.get('/', (req, res) => {
  res.render('index', { title: 'Home Page', users });
});
```

## Complete Example

```javascript
const express = require('express');
const path = require('path');
require('dotenv').config();

const app = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.static(path.join(__dirname, 'public')));

// Routes
app.get('/', (req, res) => {
  res.json({ message: 'Welcome to the API' });
});

app.get('/api/status', (req, res) => {
  res.json({
    status: 'ok',
    timestamp: new Date().toISOString(),
    uptime: process.uptime(),
  });
});

app.get('/api/users', (req, res) => {
  res.json([
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' },
  ]);
});

// 404 handler
app.use((req, res) => {
  res.status(404).json({ error: 'Not found' });
});

// Error handler
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Internal server error' });
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

## Common Use Cases

- REST API backends
- Web applications
- Microservices
- Real-time applications (with Socket.io)
- Proxy servers
- Content Management Systems
- E-commerce platforms

## Common Mistakes

### 1. Not Installing Express
```bash
# Bad - Express not installed
node app.js

# Good - Install first
npm install express
```

### 2. Not Handling 404 Routes
```javascript
// Bad - No 404 handler
app.get('/users', handler);

// Good - Add catch-all 404
app.use((req, res) => {
  res.status(404).json({ error: 'Not found' });
});
```

### 3. Forgetting Error Handling Middleware
```javascript
// Bad - Errors crash server
app.get('/risky', (req, res) => {
  throw new Error('Oops');
});

// Good - Add error handler
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.message });
});
```

### 4. Not Using Environment Variables
```javascript
// Bad - Hardcoded secrets
const dbPassword = 'mypassword123';

// Good - Use environment variables
const dbPassword = process.env.DB_PASSWORD;
```

## Quick Revision

| Step | Code |
|------|------|
| Install | `npm install express` |
| Import | `const app = express()` |
| Define routes | `app.get()`, `app.post()` |
| Add middleware | `app.use(middleware)` |
| Start server | `app.listen(port)` |

| Method | Purpose |
|--------|---------|
| `express.json()` | Parse JSON bodies |
| `express.urlencoded()` | Parse URL-encoded bodies |
| `express.static()` | Serve static files |
| `res.json()` | Send JSON response |
| `res.send()` | Send text/HTML response |
| `res.status()` | Set status code |
| `res.render()` | Render template |

## Related Topics

- [[What-is-Express]] - Express framework introduction
- [[What-is-Routes]] - Routing in Express
- [[Handle-Methods]] - Handling HTTP methods
- [[What-is-Middleware]] - Middleware concepts
- [[Serve-Static]] - Serving static files
- [[What-is-Node]] - Node.js fundamentals
- [[Create-Server]] - Raw HTTP servers
- [[What-is-HTTP]] - HTTP module

---

**Key Takeaway:** Express simplifies Node.js web development by providing a clean API for routing, middleware, and request/response handling. Start with a basic structure and expand as needed.
