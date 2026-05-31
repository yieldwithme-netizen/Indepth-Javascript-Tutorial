# How to Handle Errors in Express

## Definition

Error handling in Express involves catching and managing errors that occur during request processing. Express provides a built-in error handling mechanism using middleware functions with four parameters: `(err, req, res, next)`.

```javascript
// Error handling middleware signature
app.use((err, req, res, next) => {
  // Handle error
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

## Basic Error Handling

```javascript
const express = require('express');
const app = express();

app.use(express.json());

// Route that might throw an error
app.get('/user/:id', (req, res, next) => {
  const user = findUser(req.params.id);
  
  if (!user) {
    const err = new Error('User not found');
    err.status = 404;
    return next(err); // Pass error to error handler
  }
  
  res.json(user);
});

// Error handling middleware (must be last)
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.status || 500).json({
    error: {
      message: err.message || 'Internal Server Error',
      status: err.status || 500
    }
  });
});

app.listen(3000);
```

## Custom Error Classes

```javascript
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.status = `${statusCode}`.startsWith('4') ? 'fail' : 'error';
    this.isOperational = true;
    
    Error.captureStackTrace(this, this.constructor);
  }
}

class NotFoundError extends AppError {
  constructor(message = 'Resource not found') {
    super(message, 404);
  }
}

class ValidationError extends AppError {
  constructor(message = 'Validation failed') {
    super(message, 400);
  }
}

class UnauthorizedError extends AppError {
  constructor(message = 'Unauthorized') {
    super(message, 401);
  }
}

// Usage
app.get('/user/:id', (req, res, next) => {
  const user = findUser(req.params.id);
  
  if (!user) {
    return next(new NotFoundError('User not found'));
  }
  
  res.json(user);
});

app.post('/user', (req, res, next) => {
  if (!req.body.email) {
    return next(new ValidationError('Email is required'));
  }
  
  // Create user...
});
```

## Async Error Handling

```javascript
// Express 4.x - Wrap async functions
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

app.get('/user/:id', asyncHandler(async (req, res) => {
  const user = await User.findById(req.params.id);
  
  if (!user) {
    throw new NotFoundError('User not found');
  }
  
  res.json(user);
}));

// Express 5.x - Async errors are automatically caught
app.get('/user/:id', async (req, res) => {
  const user = await User.findById(req.params.id);
  
  if (!user) {
    throw new NotFoundError('User not found');
  }
  
  res.json(user);
});
```

## Centralized Error Handler

```javascript
// utils/errorHandler.js
const errorHandler = (err, req, res, next) => {
  // Log error
  console.error('Error:', {
    message: err.message,
    stack: err.stack,
    url: req.originalUrl,
    method: req.method,
    timestamp: new Date().toISOString()
  });

  // Determine error type
  let statusCode = err.statusCode || 500;
  let message = err.message || 'Internal Server Error';

  // Handle specific error types
  if (err.name === 'ValidationError') {
    statusCode = 400;
    message = Object.values(err.errors).map(e => e.message).join(', ');
  }

  if (err.name === 'CastError') {
    statusCode = 400;
    message = 'Invalid ID format';
  }

  if (err.code === 11000) {
    statusCode = 400;
    message = 'Duplicate field value';
  }

  // Send response
  res.status(statusCode).json({
    success: false,
    error: {
      message,
      status: statusCode,
      ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    }
  });
};

module.exports = errorHandler;
```

## Try-Catch in Route Handlers

```javascript
// Synchronous error handling
app.get('/user/:id', (req, res) => {
  try {
    const user = findUser(req.params.id);
    
    if (!user) {
      throw new AppError('User not found', 404);
    }
    
    res.json(user);
  } catch (err) {
    next(err); // Pass to error handler
  }
});

// Async error handling with try-catch
app.get('/user/:id', async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    
    if (!user) {
      throw new AppError('User not found', 404);
    }
    
    res.json(user);
  } catch (err) {
    next(err);
  }
});
```

## Express Error Handling Middleware Placement

```javascript
const express = require('express');
const app = express();

// 1. Regular middleware
app.use(express.json());
app.use(morgan('dev'));

// 2. Routes
app.get('/', (req, res) => {
  res.send('Home');
});

app.get('/error', (req, res) => {
  throw new Error('Something went wrong');
});

// 3. 404 handler (after all routes)
app.use((req, res, next) => {
  const err = new Error('Not Found');
  err.status = 404;
  next(err);
});

// 4. Error handler (must be last)
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.status || 500).json({
    error: {
      message: err.message,
      status: err.status || 500
    }
  });
});

app.listen(3000);
```

## Common Use Cases

- **404 Not Found**: Handle requests to non-existent routes
- **Validation Errors**: Handle invalid request data
- **Database Errors**: Handle database connection and query errors
- **Authentication Errors**: Handle unauthorized access attempts
- **File Upload Errors**: Handle file size limits and type validation
- **External API Errors**: Handle failures from third-party services

## Common Mistakes

1. **Missing `next()` in error middleware**: Always call `next(err)` to pass errors
2. **Not returning after `next(err)`**: Execution continues if you don't return
3. **Error handler not last**: Error middleware must come after all routes
4. **Not handling async errors**: Async errors need special handling in Express 4
5. **Exposing stack traces**: Don't send stack traces to clients in production

```javascript
// WRONG - Error handler placed before routes
app.use((err, req, res, next) => {
  res.status(500).send('Error');
});

app.get('/', (req, res) => {
  throw new Error('Oops');
});

// CORRECT - Error handler placed after routes
app.get('/', (req, res) => {
  throw new Error('Oops');
});

app.use((err, req, res, next) => {
  res.status(500).send('Error');
});

// WRONG - Not returning after next(err)
app.get('/user', (req, res, next) => {
  if (!req.query.id) {
    next(new Error('No ID'));
    // Execution continues!
  }
  // This code runs even after error
});

// CORRECT
app.get('/user', (req, res, next) => {
  if (!req.query.id) {
    return next(new Error('No ID'));
  }
  // This code only runs if no error
});
```

## Quick Revision

- Error middleware has 4 parameters: `(err, req, res, next)`
- Always place error handlers after all routes
- Create custom error classes for different error types
- Use `next(err)` to pass errors to the error handler
- Return after calling `next(err)` to prevent further execution
- Handle async errors with async handlers or try-catch
- Never expose stack traces in production
- Centralize error handling logic in one place

## Related Topics

- [[Use-Middleware]]
- [[What-is-Router]]
- [[What-is-BodyParser]]
- [[Express-Basics]]
- [[Logging]]
