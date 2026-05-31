# What is the http Module?

The **http** module is a built-in Node.js module that provides utilities for creating HTTP servers and making HTTP requests. It's the foundation for building web applications and APIs in Node.js.

## Definition

The `http` module allows you to create HTTP servers that listen for incoming requests and send responses. It also provides a client API for making HTTP requests to other servers.

```javascript
// Import the http module
const http = require('http');

// Or with ES modules
import http from 'http';
```

## Creating a Basic HTTP Server

```javascript
const http = require('http');

// Create server
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});

// Start listening
const PORT = 3000;
server.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}/`);
});
```

## Understanding Request and Response

### Request Object (req)

```javascript
const server = http.createServer((req, res) => {
  // Request properties
  console.log('Method:', req.method);           // GET, POST, PUT, DELETE
  console.log('URL:', req.url);                 // /api/users
  console.log('Headers:', req.headers);         // { host: '...', ... }

  // Request URL
  const url = new URL(req.url, `http://${req.headers.host}`);
  console.log('Pathname:', url.pathname);
  console.log('Query:', url.searchParams);

  res.end('OK');
});
```

### Response Object (res)

```javascript
const server = http.createServer((req, res) => {
  // Set status code
  res.statusCode = 200;

  // Set headers
  res.setHeader('Content-Type', 'application/json');
  res.setHeader('X-Custom-Header', 'value');

  // Send response
  res.end(JSON.stringify({ message: 'Success' }));
});
```

## Handling Different Routes

```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url, true);
  const path = parsedUrl.pathname;

  // Route handling
  if (path === '/') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('<h1>Home Page</h1>');
  } else if (path === '/about') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('<h1>About Page</h1>');
  } else if (path === '/api/data') {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ users: ['Alice', 'Bob'] }));
  } else {
    res.writeHead(404, { 'Content-Type': 'text/plain' });
    res.end('404 Not Found');
  }
});

server.listen(3000);
```

## Handling HTTP Methods

```javascript
const server = http.createServer((req, res) => {
  res.setHeader('Content-Type', 'application/json');

  switch (req.method) {
    case 'GET':
      handleGet(req, res);
      break;
    case 'POST':
      handlePost(req, res);
      break;
    case 'PUT':
      handlePut(req, res);
      break;
    case 'DELETE':
      handleDelete(req, res);
      break;
    default:
      res.writeHead(405);
      res.end(JSON.stringify({ error: 'Method not allowed' }));
  }
});

function handleGet(req, res) {
  res.writeHead(200);
  res.end(JSON.stringify({ message: 'GET request' }));
}

function handlePost(req, res) {
  let body = '';

  req.on('data', (chunk) => {
    body += chunk.toString();
  });

  req.on('end', () => {
    const data = JSON.parse(body);
    res.writeHead(201);
    res.end(JSON.stringify({ message: 'Created', data }));
  });
}
```

## Serving Static Files

```javascript
const http = require('http');
const fs = require('fs');
const path = require('path');

const server = http.createServer((req, res) => {
  let filePath = path.join(__dirname, 'public', req.url === '/' ? 'index.html' : req.url);

  const ext = path.extname(filePath);
  const contentType = {
    '.html': 'text/html',
    '.css': 'text/css',
    '.js': 'application/javascript',
    '.json': 'application/json',
  }[ext] || 'text/plain';

  fs.readFile(filePath, (err, content) => {
    if (err) {
      res.writeHead(404);
      res.end('File not found');
      return;
    }

    res.writeHead(200, { 'Content-Type': contentType });
    res.end(content);
  });
});

server.listen(3000);
```

## Making HTTP Requests (Client)

```javascript
const http = require('http');

// Simple GET request
http.get('http://jsonplaceholder.typicode.com/posts/1', (res) => {
  let data = '';

  res.on('data', (chunk) => {
    data += chunk;
  });

  res.on('end', () => {
    console.log(JSON.parse(data));
  });
}).on('error', (err) => {
  console.error('Error:', err.message);
});
```

### Using http.request for POST

```javascript
const http = require('http');

const postData = JSON.stringify({
  title: 'New Post',
  body: 'Content here',
  userId: 1,
});

const options = {
  hostname: 'jsonplaceholder.typicode.com',
  port: 80,
  path: '/posts',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Content-Length': Buffer.byteLength(postData),
  },
};

const req = http.request(options, (res) => {
  let body = '';
  res.on('data', (chunk) => body += chunk);
  res.on('end', () => console.log(JSON.parse(body)));
});

req.write(postData);
req.end();
```

## Streaming Responses

```javascript
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
  if (req.url === '/video') {
    const videoPath = './video.mp4';
    const stat = fs.statSync(videoPath);

    res.writeHead(200, {
      'Content-Type': 'video/mp4',
      'Content-Length': stat.size,
    });

    const readStream = fs.createReadStream(videoPath);
    readStream.pipe(res);
  }
});

server.listen(3000);
```

## Common Use Cases

- Building REST APIs
- Creating web servers
- Proxy servers
- File download servers
- Real-time applications (with WebSocket upgrade)
- Microservices communication
- Load testing clients

## Common Mistakes

### 1. Not Setting Content-Type
```javascript
// Bad - Browser may not parse correctly
res.writeHead(200);

// Good - Always set content type
res.writeHead(200, { 'Content-Type': 'application/json' });
```

### 2. Forgetting to End Response
```javascript
// Bad - Response hangs
res.writeHead(200);
res.write('Hello');

// Good - Always end response
res.writeHead(200);
res.end('Hello');
```

### 3. Not Handling Errors in Client Requests
```javascript
// Bad - No error handling
http.get(url, (res) => { ... });

// Good - Handle errors
http.get(url, (res) => { ... }).on('error', (err) => {
  console.error('Request failed:', err);
});
```

### 4. Not Parsing Request Body
```javascript
// Bad - Body is not available directly
const data = req.body; // undefined

// Good - Collect body data
let body = '';
req.on('data', (chunk) => body += chunk);
req.on('end', () => {
  const data = JSON.parse(body);
});
```

## Quick Revision

| Component | Description |
|-----------|-------------|
| `http.createServer()` | Creates HTTP server |
| `server.listen()` | Starts listening for requests |
| `req.method` | HTTP method (GET, POST, etc.) |
| `req.url` | Request URL path |
| `req.headers` | Request headers |
| `res.statusCode` | HTTP response status |
| `res.setHeader()` | Set response header |
| `res.writeHead()` | Write status + headers |
| `res.write()` | Write response body |
| `res.end()` | End response |
| `http.request()` | Make HTTP request (client) |
| `http.get()` | Convenience GET request |

## Related Topics

- [[Create-Server]] - Building HTTP servers step-by-step
- [[What-is-Node]] - Node.js fundamentals
- [[What-is-FS]] - File system for serving files
- [[What-is-Path]] - Path handling
- [[What-is-EventLoop]] - Understanding async operations
- [[What-is-Streams]] - Stream processing for large responses

---

**Key Takeaway:** The `http` module is the backbone of Node.js web development. For production applications, use frameworks like Express.js to simplify routing and middleware handling.
