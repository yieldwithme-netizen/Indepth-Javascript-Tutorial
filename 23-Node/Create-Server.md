# How to Create an HTTP Server

This guide walks you through creating HTTP servers in Node.js, from basic to advanced implementations.

## Definition

An HTTP server listens for incoming HTTP requests and sends responses back to clients (browsers, mobile apps, other servers). Node.js makes it easy to create servers using the built-in `http` module.

## Basic HTTP Server

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, World!\n');
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```

## Server with Multiple Routes

```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url, true);
  const path = parsedUrl.pathname;

  // CORS headers
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

  // Handle preflight requests
  if (req.method === 'OPTIONS') {
    res.writeHead(204);
    res.end();
    return;
  }

  // Route: Home
  if (path === '/' && req.method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end(`
      <h1>Welcome to My Server</h1>
      <a href="/about">About</a>
      <a href="/api/users">API Users</a>
    `);
  }

  // Route: About
  else if (path === '/about' && req.method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('<h1>About Page</h1>');
  }

  // Route: API - Get users
  else if (path === '/api/users' && req.method === 'GET') {
    const users = [
      { id: 1, name: 'Alice' },
      { id: 2, name: 'Bob' },
    ];

    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify(users));
  }

  // Route: API - Create user
  else if (path === '/api/users' && req.method === 'POST') {
    let body = '';

    req.on('data', (chunk) => {
      body += chunk.toString();
    });

    req.on('end', () => {
      try {
        const newUser = JSON.parse(body);
        newUser.id = Date.now();

        res.writeHead(201, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify(newUser));
      } catch (error) {
        res.writeHead(400, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ error: 'Invalid JSON' }));
      }
    });
  }

  // 404 Not Found
  else {
    res.writeHead(404, { 'Content-Type': 'text/plain' });
    res.end('404 - Page Not Found');
  }
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

## Server with Static File Serving

```javascript
const http = require('http');
const fs = require('fs');
const path = require('path');

const MIME_TYPES = {
  '.html': 'text/html',
  '.css': 'text/css',
  '.js': 'application/javascript',
  '.json': 'application/json',
  '.png': 'image/png',
  '.jpg': 'image/jpeg',
  '.gif': 'image/gif',
  '.svg': 'image/svg+xml',
};

const PUBLIC_DIR = path.join(__dirname, 'public');

const server = http.createServer((req, res) => {
  // Build file path
  let filePath = path.join(PUBLIC_DIR, req.url === '/' ? 'index.html' : req.url);

  // Get file extension
  const ext = path.extname(filePath).toLowerCase();
  const contentType = MIME_TYPES[ext] || 'application/octet-stream';

  // Read and serve file
  fs.readFile(filePath, (err, data) => {
    if (err) {
      if (err.code === 'ENOENT') {
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('404 - File Not Found');
      } else {
        res.writeHead(500, { 'Content-Type': 'text/plain' });
        res.end('500 - Internal Server Error');
      }
      return;
    }

    res.writeHead(200, { 'Content-Type': contentType });
    res.end(data);
  });
});

server.listen(3000, () => {
  console.log('Static server running on http://localhost:3000');
});
```

## Server with Request Body Parsing

```javascript
const http = require('http');
const url = require('url');

function parseBody(req) {
  return new Promise((resolve, reject) => {
    let body = '';

    req.on('data', (chunk) => {
      body += chunk.toString();
    });

    req.on('end', () => {
      try {
        resolve(body ? JSON.parse(body) : {});
      } catch (error) {
        reject(new Error('Invalid JSON'));
      }
    });

    req.on('error', reject);
  });
}

const server = http.createServer(async (req, res) => {
  const parsedUrl = url.parse(req.url, true);
  const path = parsedUrl.pathname;

  try {
    if (path === '/api/data' && req.method === 'POST') {
      const body = await parseBody(req);

      res.writeHead(201, { 'Content-Type': 'application/json' });
      res.end(JSON.stringify({
        message: 'Data received',
        data: body,
      }));
    } else {
      res.writeHead(404, { 'Content-Type': 'application/json' });
      res.end(JSON.stringify({ error: 'Not found' }));
    }
  } catch (error) {
    res.writeHead(400, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ error: error.message }));
  }
});

server.listen(3000);
```

## Server with Middleware Pattern

```javascript
const http = require('http');

// Simple middleware system
function createServer() {
  const middlewares = [];

  const server = http.createServer(async (req, res) => {
    // Execute middlewares sequentially
    for (const middleware of middlewares) {
      const shouldContinue = await middleware(req, res);
      if (shouldContinue === false) return;
    }
  });

  return {
    use: (middleware) => middlewares.push(middleware),
    listen: (...args) => server.listen(...args),
  };
}

// Logger middleware
function logger(req, res) {
  console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
  return true; // Continue to next middleware
}

// Auth middleware
function auth(req, res) {
  const token = req.headers.authorization;

  if (req.url.startsWith('/api/') && token !== 'Bearer secret-token') {
    res.writeHead(401, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ error: 'Unauthorized' }));
    return false; // Stop processing
  }

  return true; // Continue to next middleware
}

// Routes middleware
function routes(req, res) {
  if (req.url === '/') {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Home Page');
    return false;
  }
  return true;
}

// Setup server
const app = createServer();
app.use(logger);
app.use(auth);
app.use(routes);
app.listen(3000);
```

## Server with Streaming

```javascript
const http = require('http');
const fs = require('fs');
const path = require('path');

const server = http.createServer((req, res) => {
  if (req.url === '/stream') {
    const filePath = path.join(__dirname, 'large-file.txt');
    const stat = fs.statSync(filePath);

    res.writeHead(200, {
      'Content-Type': 'text/plain',
      'Content-Length': stat.size,
    });

    const readStream = fs.createReadStream(filePath);
    readStream.pipe(res);

    readStream.on('error', (err) => {
      res.writeHead(500);
      res.end('Error reading file');
    });
  }

  if (req.url === '/progress') {
    res.writeHead(200, { 'Content-Type': 'text/plain' });

    let count = 0;
    const interval = setInterval(() => {
      res.write(`Progress: ${count}%\n`);
      count += 10;

      if (count > 100) {
        clearInterval(interval);
        res.end('Complete!');
      }
    }, 500);
  }
});

server.listen(3000);
```

## Server with HTTPS Support

```javascript
const https = require('https');
const fs = require('fs');

const options = {
  key: fs.readFileSync('private-key.pem'),
  cert: fs.readFileSync('certificate.pem'),
};

const server = https.createServer(options, (req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Secure Hello!\n');
});

server.listen(443, () => {
  console.log('HTTPS server running on https://localhost');
});
```

## Common Use Cases

- REST API backends
- Real-time applications
- File upload/download servers
- Proxy servers
- Microservices
- Development servers
- Webhook handlers

## Common Mistakes

### 1. Not Handling All HTTP Methods
```javascript
// Bad - Only handles GET
if (path === '/api/users') {
  // Handle request
}

// Good - Check method explicitly
if (path === '/api/users' && req.method === 'GET') {
  // Handle GET
} else if (path === '/api/users' && req.method === 'POST') {
  // Handle POST
}
```

### 2. Not Setting Port Properly
```javascript
// Bad - Hardcoded port
server.listen(3000);

// Good - Use environment variable
const PORT = process.env.PORT || 3000;
server.listen(PORT);
```

### 3. Not Handling Errors
```javascript
// Bad - Server crashes on error
server.listen(3000);

// Good - Handle server errors
server.on('error', (err) => {
  console.error('Server error:', err);
});

process.on('uncaughtException', (err) => {
  console.error('Uncaught exception:', err);
  process.exit(1);
});
```

## Quick Revision

| Step | Code |
|------|------|
| Import module | `const http = require('http')` |
| Create server | `http.createServer(callback)` |
| Handle request | Check `req.method` and `req.url` |
| Send response | Use `res.writeHead()` and `res.end()` |
| Start server | `server.listen(port, callback)` |
| Handle errors | Add error event listeners |

## Related Topics

- [[What-is-HTTP]] - HTTP module fundamentals
- [[What-is-Node]] - Node.js introduction
- [[What-is-FS]] - File system for static files
- [[What-is-Path]] - Path handling
- [[What-is-EventLoop]] - Understanding async
- [[What-is-Express]] - Express framework (easier way)
- [[Create-App]] - Creating Express apps

---

**Key Takeaway:** While the built-in `http` module gives you full control, frameworks like Express simplify routing, middleware, and common patterns. Use raw `http` for learning or custom requirements, Express for production apps.
