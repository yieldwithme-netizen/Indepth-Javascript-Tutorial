# What is Middleware in Express?

**Middleware** functions are functions that have access to the request object (`req`), response object (`res`), and the next middleware function in the application's request-response cycle. They can execute code, modify request/response objects, end the cycle, or call the next middleware.

## Definition

Middleware is code that runs between receiving a request and sending a response. It can perform tasks like authentication, logging, parsing, validation, error handling, and more.

```javascript
// Basic middleware structure
function middleware(req, res, next) {
  // Do something
  next(); // Call next middleware
}
```

## Types of Middleware

### 1. Application-Level Middleware

```javascript
const express = require('express');
const app = express();

// Function middleware
app.use((req, res, next) => {
  console.log('Time:', Date.now());
  next();
});

// Path-specific middleware
app.use('/api', (req, res, next) => {
  console.log('API request');
  next();
});

// Multiple handlers
app.use('/api',
  (req, res, next) => {
    console.log('First handler');
    next();
  },
  (req, res, next) => {
    console.log('Second handler');
    next();
  }
);
```

### 2. Router-Level Middleware

```javascript
const express = require('express');
const router = express.Router();

// Router middleware
router.use((req, res, next) => {
  console.log('Router middleware');
  next();
});

// Apply to specific routes
router.get('/users', authMiddleware, (req, res) => {
  res.json([]);
});

module.exports = router;
```

### 3. Built-in Middleware

```javascript
const express = require('express');
const app = express();

// Parse JSON
app.use(express.json());

// Parse URL-encoded
app.use(express.urlencoded({ extended: true }));

// Serve static files
app.use(express.static('public'));

// Cookie parser (requires separate package)
const cookieParser = require('cookie-parser');
app.use(cookieParser());
```

### 4. Third-Party Middleware

```javascript
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');

const app = express();

// CORS
app.use(cors());

// Security headers
app.use(helmet());

// Request logging
app.use(morgan('combined'));
```

## Common Middleware Patterns

### Logger Middleware

```javascript
function logger(req, res, next) {
  const start = Date.now();

  // Log on response finish
  res.on('finish', () => {
    const duration = Date.now() - start;
    console.log(
      `${req.method} ${req.originalUrl} ${res.statusCode} ${duration}ms`
    );
  });

  next();
}

app.use(logger);
```

### Authentication Middleware

```javascript
function authenticate(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];

  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }

  try {
    const decoded = verifyToken(token); // Your verification logic
    req.user = decoded;
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
}

// Usage
app.get('/protected', authenticate, (req, res) => {
  res.json({ message: 'Protected data', user: req.user });
});
```

### Authorization Middleware

```javascript
function authorize(...roles) {
  return (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({ error: 'Not authenticated' });
    }

    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Insufficient permissions' });
    }

    next();
  };
}

// Usage
app.get('/admin', authenticate, authorize('admin'), (req, res) => {
  res.json({ message: 'Admin panel' });
});

app.get('/moderator', authenticate, authorize('admin', 'moderator'), (req, res) => {
  res.json({ message: 'Moderator panel' });
});
```

### Validation Middleware

```javascript
function validate(schema) {
  return (req, res, next) => {
    const { error, value } = schema.validate(req.body);

    if (error) {
      return res.status(400).json({
        error: error.details.map(d => d.message),
      });
    }

    req.body = value;
    next();
  };
}

// Usage with Joi
const Joi = require('joi');

const userSchema = Joi.object({
  name: Joi.string().min(2).required(),
  email: Joi.string().email().required(),
  age: Joi.number().min(18).optional(),
});

app.post('/users', validate(userSchema), (req, res) => {
  // req.body is validated and sanitized
  res.status(201).json(req.body);
});
```

### Error Handling Middleware

```javascript
// Error handling middleware (4 parameters)
function errorHandler(err, req, res, next) {
  console.error(err.stack);

  const status = err.status || 500;
  const message = err.message || 'Internal Server Error';

  res.status(status).json({
    error: {
      message,
      ...(process.env.NODE_ENV === 'development' && { stack: err.stack }),
    },
  });
}

// Usage
app.use(errorHandler);

// Creating errors
app.get('/fail', (req, res, next) => {
  const error = new Error('Something went wrong');
  error.status = 500;
  next(error);
});
```

### Rate Limiting Middleware

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
  message: 'Too many requests',
  standardHeaders: true,
  legacyHeaders: false,
});

app.use('/api', limiter);

// Stricter limit for auth routes
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5,
  message: 'Too many login attempts',
});

app.use('/auth/login', authLimiter);
```

### Body Parser Middleware

```javascript
const express = require('express');
const multer = require('multer');

const app = express();

// JSON parser
app.use(express.json());

// URL-encoded parser
app.use(express.urlencoded({ extended: true }));

// File upload parser
const upload = multer({ dest: 'uploads/' });

app.post('/upload', upload.single('file'), (req, res) => {
  console.log(req.file); // Uploaded file info
  res.json({ file: req.file });
});

app.post('/uploads', upload.array('files', 5), (req, res) => {
  console.log(req.files); // Array of uploaded files
  res.json({ files: req.files });
});
```

## Middleware Chain

```javascript
const express = require('express');
const app = express();

// Middleware 1: Log request
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next(); // MUST call next() or send response
});

// Middleware 2: Parse auth token
app.use((req, res, next) => {
  const token = req.headers.authorization;
  req.user = decodeToken(token);
  next();
});

// Middleware 3: Check permission
function requireAuth(req, res, next) {
  if (!req.user) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
  next();
}

// Route with multiple middleware
app.get('/profile',
  requireAuth,           // First: check auth
  (req, res) => {        // Then: handle request
    res.json(req.user);
  }
);
```

## Complete Example

```javascript
const express = require('express');
const app = express();

// Body parsing
app.use(express.json());

// Request logging
app.use((req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
  next();
});

// CORS
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
  res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization');
  next();
});

// Auth middleware
const authenticate = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
  req.user = { id: 1, role: 'user' }; // Verify token here
  next();
};

// Routes
app.get('/public', (req, res) => {
  res.json({ message: 'Public data' });
});

app.get('/private', authenticate, (req, res) => {
  res.json({ message: 'Private data', user: req.user });
});

// 404 handler
app.use((req, res) => {
  res.status(404).json({ error: 'Not found' });
});

// Error handler
app.use((err, req, res, next) => {
  console.error(err);
  res.status(500).json({ error: 'Internal server error' });
});

app.listen(3000);
```

## Common Use Cases

- Authentication and authorization
- Request logging
- Body parsing
- CORS handling
- Rate limiting
- Input validation
- Error handling
- Caching
- Compression
- Security headers

## Common Mistakes

### 1. Forgetting to Call next()
```javascript
// Bad - Request hangs
app.use((req, res, next) => {
  console.log('Middleware');
  // Missing next()
});

// Good - Always call next()
app.use((req, res, next) => {
  console.log('Middleware');
  next();
});
```

### 2. Not Sending Response or Calling next
```javascript
// Bad - Request hangs
app.use((req, res, next) => {
  if (error) {
    res.status(500).json({ error: 'Error' });
    // Missing return
  }
  next();
});

// Good - Return after sending response
app.use((req, res, next) => {
  if (error) {
    return res.status(500).json({ error: 'Error' });
  }
  next();
});
```

### 3. Wrong Error Handler Signature
```javascript
// Bad - Missing error parameter
app.use((req, res, next) => {
  res.status(500).json({ error: 'Error' });
});

// Good - 4 parameters for error handler
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({ error: err.message });
});
```

### 4. Middleware Order Issues
```javascript
// Bad - Body parser after route
app.get('/data', (req, res) => {
  console.log(req.body); // undefined
});
app.use(express.json());

// Good - Body parser before routes
app.use(express.json());
app.get('/data', (req, res) => {
  console.log(req.body); // parsed body
});
```

## Quick Revision

| Middleware Type | Purpose | Example |
|-----------------|---------|---------|
| Application | Runs on all routes | `app.use()` |
| Router | Runs on router routes | `router.use()` |
| Built-in | Express features | `express.json()` |
| Third-party | Community packages | `cors()`, `helmet()` |
| Error | Handles errors | `(err, req, res, next)` |

| Task | Middleware |
|------|------------|
| Logging | `morgan`, custom logger |
| Auth | `passport`, custom authenticate |
| CORS | `cors` |
| Security | `helmet` |
| Validation | `joi`, `express-validator` |
| Rate Limiting | `express-rate-limit` |
| Body Parsing | `express.json()`, `multer` |

## Related Topics

- [[What-is-Express]] - Express framework basics
- [[What-is-Routes]] - Route definitions
- [[Handle-Methods]] - HTTP method handling
- [[Create-App]] - Setting up Express apps
- [[What-is-HTTP]] - HTTP fundamentals
- [[What-is-Node]] - Node.js basics

---

**Key Takeaway:** Middleware is the backbone of Express applications. It enables separation of concerns and code reuse. Always call `next()` unless you're ending the request cycle, and be careful with middleware order.
