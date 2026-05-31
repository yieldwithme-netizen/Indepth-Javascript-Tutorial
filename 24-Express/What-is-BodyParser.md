# What is Body-Parser?

## Definition

Body-parser is middleware that parses incoming request bodies in a middleware before your handlers, available under the `req.body` property. It extracts the entire body of an incoming request stream and exposes it on `req.body` as a JSON object.

```javascript
// Body-parser is now built into Express
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
```

## Why Body-Parser is Needed

By default, Express does not parse request bodies. Without body-parser, `req.body` will be `undefined`.

```javascript
// WITHOUT body-parser
app.post('/user', (req, res) => {
  console.log(req.body); // undefined!
  res.send('Received');
});

// WITH body-parser
app.use(express.json());

app.post('/user', (req, res) => {
  console.log(req.body); // { name: "Alice", email: "alice@example.com" }
  res.send('Received');
});
```

## Built-in Body Parsers in Express

### JSON Parser

```javascript
// Parse JSON request bodies
app.use(express.json());

// With options
app.use(express.json({ 
  limit: '10mb',           // Maximum request body size
  type: 'application/json' // Content type to parse
}));

// Usage
app.post('/api/data', (req, res) => {
  console.log(req.body);
  res.json({ received: req.body });
});
```

### URL-Encoded Parser

```javascript
// Parse URL-encoded bodies (form data)
app.use(express.urlencoded({ extended: true }));

// With options
app.use(express.urlencoded({ 
  extended: true,    // Use qs library for parsing
  limit: '10mb',     // Maximum request body size
  parameterLimit: 1000 // Maximum number of parameters
}));

// Usage
app.post('/form', (req, res) => {
  console.log(req.body); // { username: "alice", password: "12345" }
  res.send('Form received');
});
```

## Request Content Types

### JSON Bodies

```javascript
// Client sends: Content-Type: application/json
// Request body: {"name": "Alice", "age": 25}

app.use(express.json());

app.post('/json', (req, res) => {
  console.log(req.body.name);  // "Alice"
  console.log(req.body.age);   // 25
  res.json({ received: true });
});
```

### URL-Encoded Bodies

```javascript
// Client sends: Content-Type: application/x-www-form-urlencoded
// Request body: name=Alice&age=25

app.use(express.urlencoded({ extended: true }));

app.post('/form', (req, res) => {
  console.log(req.body.name);  // "Alice"
  console.log(req.body.age);   // "25" (string)
  res.json({ received: true });
});
```

### Raw Bodies

```javascript
// For raw data (binary, text, etc.)
app.use(express.raw({ type: '*/*' }));

app.post('/raw', (req, res) => {
  console.log(req.body); // Buffer
  res.send('Raw data received');
});
```

### Text Bodies

```javascript
// For plain text
app.use(express.text({ type: 'text/plain' }));

app.post('/text', (req, res) => {
  console.log(req.body); // String
  res.send('Text received');
});
```

## Handling Multiple Content Types

```javascript
// Parse multiple content types
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Middleware to handle different content types
const parseBody = (req, res, next) => {
  const contentType = req.headers['content-type'];
  
  if (contentType && contentType.includes('application/json')) {
    // JSON body is already parsed
    next();
  } else if (contentType && contentType.includes('application/x-www-form-urlencoded')) {
    // URL-encoded body is already parsed
    next();
  } else {
    next();
  }
};

app.use(parseBody);
```

## Practical Example: User Registration

```javascript
const express = require('express');
const app = express();

// Parse JSON and URL-encoded bodies
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// User registration endpoint
app.post('/register', (req, res) => {
  const { username, email, password, age } = req.body;
  
  // Validation
  if (!username || !email || !password) {
    return res.status(400).json({ 
      error: 'Username, email, and password are required' 
    });
  }
  
  if (age && (age < 0 || age > 150)) {
    return res.status(400).json({ 
      error: 'Age must be between 0 and 150' 
    });
  }
  
  // Create user (simplified)
  const user = {
    id: Date.now(),
    username,
    email,
    age: age || null,
    createdAt: new Date()
  };
  
  res.status(201).json({ 
    message: 'User registered successfully',
    user 
  });
});

app.listen(3000);
```

## Body-Parser Options

```javascript
// JSON parser options
app.use(express.json({
  limit: '10mb',                    // Limit request size
  type: 'application/json',         // Content type to parse
  strict: true,                     // Only parse objects and arrays
  verify: (req, res, buf) => {      // Verify callback
    // Store raw body for webhook verification
    req.rawBody = buf;
  }
}));

// URL-encoded parser options
app.use(express.urlencoded({
  extended: true,                    // Use qs library
  limit: '10mb',                    // Limit request size
  parameterLimit: 1000,             // Max parameters
  type: 'application/x-www-form-urlencoded'
}));
```

## Common Use Cases

- **Form Processing**: Handle HTML form submissions
- **API Requests**: Parse JSON payloads from API clients
- **Webhook Handling**: Receive and verify webhook data
- **File Uploads**: Parse multipart form data (requires multer)
- **Search Queries**: Parse search request bodies
- **Shopping Carts**: Handle cart item data

## Common Mistakes

1. **Not adding body-parser middleware**: `req.body` will be `undefined`
2. **Wrong order**: Body-parser must be before routes that need it
3. **Missing content-type**: Client must send correct Content-Type header
4. **Large payloads**: Set appropriate size limits to prevent memory issues
5. **Not handling errors**: Invalid JSON will throw errors

```javascript
// WRONG - Body-parser after routes
app.get('/data', (req, res) => {
  console.log(req.body); // undefined
  res.json(req.body);
});

app.use(express.json()); // Too late!

// CORRECT - Body-parser before routes
app.use(express.json());

app.get('/data', (req, res) => {
  console.log(req.body); // Parsed body
  res.json(req.body);
});

// WRONG - No error handling for invalid JSON
app.use(express.json());

// CORRECT - Handle JSON parse errors
app.use(express.json());
app.use((err, req, res, next) => {
  if (err.type === 'entity.parse.failed') {
    return res.status(400).json({ error: 'Invalid JSON' });
  }
  next(err);
});
```

## Quick Revision

- Body-parser extracts request body and makes it available as `req.body`
- Built into Express 4.16+ as `express.json()` and `express.urlencoded()`
- `express.json()` parses JSON payloads (Content-Type: application/json)
- `express.urlencoded()` parses form data (Content-Type: application/x-www-form-urlencoded)
- Set `limit` option to prevent large payload attacks
- Place body-parser middleware before routes that need it
- Always handle parse errors for invalid payloads
- `extended: true` uses `qs` library for richer objects

## Related Topics

- [[Use-Middleware]]
- [[Handle-Errors]]
- [[What-is-Router]]
- [[Express-Basics]]
- [[File-Upload]]
