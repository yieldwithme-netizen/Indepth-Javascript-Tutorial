# How to Use Middleware in Express

## What is Middleware?

Middleware functions are functions that have access to the request object (`req`), the response object (`res`), and the next middleware function in the application's request-response cycle. They can execute code, modify `req` and `res` objects, end the request-response cycle, or call the next middleware in the stack.

```javascript
const express = require('express');
const app = express();

// Simple middleware function
const myMiddleware = (req, res, next) => {
  console.log('Middleware executed at:', new Date());
  next(); // Always call next() to pass control to the next middleware
};

app.use(myMiddleware);

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(3000);
```

## Types of Middleware

### Application-Level Middleware

Bound to the Express app instance using `app.use()` or `app.METHOD()`.

```javascript
// Using app.use() - applies to all routes
app.use((req, res, next) => {
  console.log('Request URL:', req.url);
  next();
});

// Using app.use() - applies to specific route
app.use('/api', (req, res, next) => {
  console.log('API route accessed');
  next();
});

// Using app.METHOD() - applies to specific HTTP method and route
app.get('/user', (req, res, next) => {
  console.log('GET /user accessed');
  next();
});
```

### Router-Level Middleware

Bound to an Express Router instance.

```javascript
const router = express.Router();

const checkAuth = (req, res, next) => {
  if (req.query.admin === 'true') {
    next();
  } else {
    res.status(403).send('Forbidden');
  }
};

router.get('/admin', checkAuth, (req, res) => {
  res.send('Welcome Admin!');
});

app.use('/dashboard', router);
```

### Built-In Middleware

Express provides built-in middleware functions:

```javascript
// Serve static files
app.use(express.static('public'));

// Parse URL-encoded bodies
app.use(express.urlencoded({ extended: true }));

// Parse JSON bodies
app.use(express.json());
```

### Third-Party Middleware

```javascript
const morgan = require('morgan');
const cors = require('cors');
const helmet = require('helmet');

// HTTP request logger
app.use(morgan('dev'));

// Enable CORS
app.use(cors());

// Secure HTTP headers
app.use(helmet());
```

## Writing Custom Middleware

```javascript
// Logger middleware
const logger = (req, res, next) => {
  const timestamp = new Date().toISOString();
  console.log(`[${timestamp}] ${req.method} ${req.url}`);
  next();
};

// Authentication middleware
const authenticate = (req, res, next) => {
  const token = req.headers.authorization;
  
  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }
  
  try {
    const decoded = jwt.verify(token, 'secretKey');
    req.user = decoded;
    next();
  } catch (err) {
    res.status(401).json({ error: 'Invalid token' });
  }
};

// Error handling middleware (4 parameters)
const errorHandler = (err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: err.message });
};

// Usage
app.use(logger);
app.get('/protected', authenticate, (req, res) => {
  res.json({ message: 'Protected data', user: req.user });
});
app.use(errorHandler);
```

## Middleware Execution Order

```javascript
// Middleware executes in order of definition
app.use((req, res, next) => {
  console.log('First middleware');
  next();
});

app.use((req, res, next) => {
  console.log('Second middleware');
  next();
});

app.get('/', (req, res) => {
  console.log('Route handler');
  res.send('Hello');
});

// Output order: First middleware -> Second middleware -> Route handler
```

## Common Use Cases

- **Authentication & Authorization**: Verify user credentials before accessing routes
- **Logging**: Log request details for debugging and monitoring
- **Body Parsing**: Parse incoming request bodies (JSON, URL-encoded, etc.)
- **CORS**: Enable Cross-Origin Resource Sharing
- **Rate Limiting**: Limit request frequency to prevent abuse
- **Error Handling**: Centralize error handling logic
- **Static Files**: Serve static assets like HTML, CSS, images

## Common Mistakes

1. **Not calling `next()`**: Forgetting to call `next()` will stop the request cycle
2. **Not ending the response**: If you don't call `next()` or send a response, the request hangs
3. **Error middleware wrong signature**: Error middleware must have 4 parameters: `(err, req, res, next)`
4. **Modifying middleware order**: Order matters - middleware executes in the order it's defined
5. **Not returning after sending response**: Always `return` after sending a response to prevent further execution

```javascript
// WRONG - Missing next()
app.use((req, res, next) => {
  console.log('Logging');
  // Forgot next() - request will hang!
});

// CORRECT
app.use((req, res, next) => {
  console.log('Logging');
  next();
});

// WRONG - Missing return after send
app.use((req, res, next) => {
  if (!req.query.token) {
    res.send('No token');
    // Execution continues!
  }
  next();
});

// CORRECT
app.use((req, res, next) => {
  if (!req.query.token) {
    return res.send('No token');
  }
  next();
});
```

## Quick Revision

- Middleware functions have access to `req`, `res`, and `next`
- Always call `next()` to pass control to the next middleware (unless ending the cycle)
- Error middleware requires 4 parameters: `(err, req, res, next)`
- Middleware executes in order of definition
- Use `app.use()` for application-level, `router.use()` for router-level
- Common uses: logging, authentication, body parsing, error handling
- Built-in middleware: `express.static()`, `express.json()`, `express.urlencoded()`

## Related Topics

- [[What-is-Router]]
- [[Handle-Errors]]
- [[What-is-BodyParser]]
- [[Express-Basics]]
- [[Authentication]]
