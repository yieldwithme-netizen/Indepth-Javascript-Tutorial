# Handle CORS

## Definition

**Handling CORS** involves configuring your server to properly respond to cross-origin requests from browsers. This includes setting appropriate headers, handling preflight requests, and managing credentials securely.

## Express.js CORS Setup

```javascript
const express = require('express');
const cors = require('cors');
const app = express();

// Basic setup - allows all origins (development only)
app.use(cors());

// Production setup
const corsOptions = {
  origin: 'https://yourdomain.com',
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  credentials: true
};
app.use(cors(corsOptions));
```

## Manual CORS Headers

```javascript
app.use((req, res, next) => {
  // Set CORS headers
  res.header('Access-Control-Allow-Origin', 'https://yourdomain.com');
  res.header('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, PATCH');
  res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization');
  res.header('Access-Control-Allow-Credentials', 'true');
  
  // Handle preflight
  if (req.method === 'OPTIONS') {
    return res.sendStatus(200);
  }
  
  next();
});
```

## Dynamic Origin Handling

```javascript
const allowedOrigins = [
  'https://yourdomain.com',
  'https://www.yourdomain.com',
  'http://localhost:3000' // Development
];

app.use((req, res, next) => {
  const origin = req.headers.origin;
  
  if (allowedOrigins.includes(origin)) {
    res.setHeader('Access-Control-Allow-Origin', origin);
  }
  
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type, Authorization');
  res.setHeader('Access-Control-Allow-Credentials', 'true');
  
  if (req.method === 'OPTIONS') {
    return res.sendStatus(200);
  }
  
  next();
});
```

## Handling Specific Routes

```javascript
// CORS for specific endpoints
app.get('/api/public', cors(), (req, res) => {
  res.json({ message: 'Public data' });
});

// Different CORS for different origins
app.get('/api/admin', (req, res) => {
  const origin = req.headers.origin;
  
  if (origin === 'https://admin.yourdomain.com') {
    res.setHeader('Access-Control-Allow-Origin', origin);
    res.json({ message: 'Admin data' });
  } else {
    res.status(403).json({ error: 'Forbidden' });
  }
});
```

## Axios Client-Side Handling

```javascript
const axios = require('axios');

// For requests with credentials
axios.get('https://api.yourdomain.com/data', {
  withCredentials: true,
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  }
});

// Proxy setup for development (in package.json)
// {
//   "proxy": "http://localhost:5000"
// }

// Or use axios defaults
axios.defaults.baseURL = 'http://localhost:5000';
axios.defaults.withCredentials = true;
```

## Fetch API Client-Side

```javascript
// Browser fetch with credentials
fetch('https://api.yourdomain.com/data', {
  method: 'GET',
  credentials: 'include', // Send cookies
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  }
});

// POST request
fetch('https://api.yourdomain.com/data', {
  method: 'POST',
  credentials: 'include',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  },
  body: JSON.stringify({ key: 'value' })
});
```

## Nginx Configuration

```nginx
# nginx.conf
server {
  listen 80;
  server_name yourdomain.com;
  
  location /api/ {
    # CORS headers
    add_header Access-Control-Allow-Origin "https://yourdomain.com" always;
    add_header Access-Control-Allow-Methods "GET, POST, PUT, DELETE, OPTIONS" always;
    add_header Access-Control-Allow-Headers "Content-Type, Authorization" always;
    add_header Access-Control-Allow-Credentials "true" always;
    
    # Handle preflight
    if ($request_method = 'OPTIONS') {
      return 204;
    }
    
    proxy_pass http://backend:5000;
  }
}
```

## Error Handling

```javascript
// Custom CORS error handler
app.use((err, req, res, next) => {
  if (err.message === 'CORS policy violation') {
    return res.status(403).json({
      error: 'CORS Error',
      message: 'Cross-origin request blocked'
    });
  }
  next(err);
});

// Log CORS errors
app.use((req, res, next) => {
  const origin = req.headers.origin;
  if (origin && !allowedOrigins.includes(origin)) {
    console.warn(`CORS blocked: ${origin} tried to access ${req.url}`);
  }
  next();
});
```

## Common Mistakes

1. **Using `*` with credentials**: Cannot use `*` with `credentials: true`
2. **Missing OPTIONS handling**: Always handle preflight requests
3. **Hardcoding origins**: Use environment variables
4. **Not logging blocked requests**: Monitor for security
5. **Forgetting to test**: Test with different browsers and origins

## Related Topics

- [[What-is-CORS]]
- [[What-is-REST]]
- [[What-is-JWT]]

## Quick Revision

- Use `cors` middleware for easy setup in Express
- Never use `*` origin in production
- Handle preflight OPTIONS requests
- Set `credentials: true` for authenticated requests
- Test with different origins and browsers
- Log blocked requests for security monitoring
