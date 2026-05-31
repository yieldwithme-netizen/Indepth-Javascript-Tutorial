# What is CORS

## Definition

**CORS (Cross-Origin Resource Sharing)** is a security mechanism that allows or restricts web applications from making requests to a different domain than the one that served the web page. It's a browser security feature that prevents malicious websites from accessing resources from other origins.

## Same-Origin Policy

```javascript
// Same Origin = same protocol + host + port
// https://example.com:443
// https://example.com:443/page1 ✅ Same origin
// http://example.com:80/page1 ❌ Different protocol
// https://api.example.com:443/page1 ❌ Different host
// https://example.com:8080/page1 ❌ Different port

// Cross-origin request blocked by browser:
fetch('https://api.otherdomain.com/data')
  .then(response => response.json())
  .catch(error => console.error('CORS error:', error));
```

## CORS Headers

```javascript
// Server-side CORS configuration
const express = require('express');
const app = express();

// Basic CORS setup
app.use((req, res, next) => {
  // Allow specific origin
  res.setHeader('Access-Control-Allow-Origin', 'https://example.com');
  
  // Allow specific methods
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, OPTIONS');
  
  // Allow specific headers
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type, Authorization');
  
  // Allow credentials
  res.setHeader('Access-Control-Allow-Credentials', 'true');
  
  // Handle preflight
  if (req.method === 'OPTIONS') {
    return res.sendStatus(200);
  }
  
  next();
});

// Or use cors middleware
const cors = require('cors');

// Allow all origins (development only)
app.use(cors());

// Allow specific origins
app.use(cors({
  origin: ['https://example.com', 'https://www.example.com'],
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  credentials: true
}));

// Dynamic origin
app.use(cors({
  origin: (origin, callback) => {
    const allowedOrigins = ['https://example.com'];
    if (!origin || allowedOrigins.includes(origin)) {
      callback(null, true);
    } else {
      callback(new Error('Not allowed by CORS'));
    }
  }
}));
```

## Preflight Requests

```javascript
// Browser sends OPTIONS request before actual request
// for non-simple requests

// Simple request: GET, POST, HEAD with standard headers
// Non-simple request: PUT, DELETE, custom headers

// Handle preflight
app.options('/api/data', cors());

// Or manually
app.options('/api/data', (req, res) => {
  res.setHeader('Access-Control-Allow-Origin', 'https://example.com');
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type, Authorization');
  res.sendStatus(200);
});
```

## Common CORS Errors

```javascript
// Error 1: No 'Access-Control-Allow-Origin' header
// Solution: Add CORS headers to server

// Error 2: Origin not allowed
// Solution: Add origin to allowed list
app.use(cors({
  origin: 'https://yourdomain.com'
}));

// Error 3: Credentials not allowed
// Solution: Enable credentials
app.use(cors({
  credentials: true
}));
app.use(express.json({ limit: '10mb' }));

// Error 4: Method not allowed
// Solution: Add method to allowed list
app.use(cors({
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH']
}));
```

## Production CORS Setup

```javascript
// Secure CORS configuration
const corsOptions = {
  origin: (origin, callback) => {
    const allowedOrigins = [
      'https://yourdomain.com',
      'https://www.yourdomain.com'
    ];
    
    // Allow requests with no origin (mobile apps, curl)
    if (!origin) return callback(null, true);
    
    if (allowedOrigins.includes(origin)) {
      callback(null, true);
    } else {
      callback(new Error('CORS policy violation'));
    }
  },
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH'],
  allowedHeaders: ['Content-Type', 'Authorization', 'X-Requested-With'],
  exposedHeaders: ['X-Total-Count'],
  credentials: true,
  maxAge: 86400 // 24 hours preflight cache
};

app.use(cors(corsOptions));
```

## Common Mistakes

1. **Using `*` in production**: Always specify allowed origins
2. **Forgetting preflight**: Handle OPTIONS requests
3. **Not handling credentials**: Set `credentials: true`
4. **Missing headers**: Include all necessary headers
5. **Ignoring error handling**: Return proper error responses

## Related Topics

- [[Handle-CORS]]
- [[What-is-REST]]
- [[What-is-JWT]]

## Quick Revision

- CORS controls cross-origin HTTP requests
- Same-origin policy blocks unauthorized requests
- Use `Access-Control-Allow-Origin` header
- Handle preflight OPTIONS requests
- Never use `*` origin in production
- Use `cors` middleware for easy setup
