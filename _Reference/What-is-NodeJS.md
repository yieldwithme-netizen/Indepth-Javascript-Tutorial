# Node.js

Node.js is a JavaScript runtime built on Chrome's V8 engine that allows you to run JavaScript on the server side.

## What is Node.js?

- JavaScript runtime (not a framework or library)
- Built on V8 JavaScript engine
- Event-driven, non-blocking I/O model
- Uses npm for package management

## Basic Example

```javascript
// server.js
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello World\n');
});

server.listen(3000, '127.0.0.1', () => {
  console.log('Server running at http://127.0.0.1:3000/');
});
```

## File System Operations

```javascript
const fs = require('fs').promises;

// Read file
async function readFile(path) {
  const data = await fs.readFile(path, 'utf8');
  return data;
}

// Write file
async function writeFile(path, content) {
  await fs.writeFile(path, content, 'utf8');
}

// List directory
async function listDir(path) {
  const files = await fs.readdir(path);
  return files;
}
```

## Modules

```javascript
// CommonJS
const math = require('./math');
module.exports = { add, subtract };

// ES Modules
import { add, subtract } from './math.js';
export const multiply = (a, b) => a * b;
```

## npm Scripts

```json
{
  "name": "my-app",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "test": "jest",
    "build": "webpack --mode production"
  }
}
```

## Express.js Example

```javascript
const express = require('express');
const app = express();

app.use(express.json());

app.get('/api/users', (req, res) => {
  res.json([{ id: 1, name: 'John' }]);
});

app.post('/api/users', (req, res) => {
  const user = req.body;
  res.status(201).json(user);
});

app.listen(3000);
```

## Common Use Cases

- REST APIs
- Real-time applications
- Microservices
- CLI tools
- Server-side rendering

## Common Mistakes

- Using blocking operations in event loop
- Not handling errors in async code
- Mixing up `__dirname` and `import.meta.url`
- Not closing database connections
- Ignoring security best practices

## Related Topics

- [[npm]]
- [[Express.js]]
- [[Modules]]
- [[Event Loop]]
- [[Streams]]

## Quick Revision

- Node.js runs JavaScript on the server
- Built on V8 engine, event-driven, non-blocking
- Use `require` (CommonJS) or `import` (ES Modules)
- npm manages packages and scripts
- Express.js simplifies API development
- Avoid blocking the event loop
