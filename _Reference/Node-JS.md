# Node-JS

## Definition

Node-JS is a **JavaScript runtime** built on Chrome's V8 engine for building server-side applications.

## Creating a Server

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello World!\n');
});

server.listen(3000, () => {
    console.log('Server running at http://localhost:3000/');
});
```

## Express.js Setup

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

## File Operations

```javascript
const fs = require('fs').promises;

// Read file
const data = await fs.readFile('file.txt', 'utf8');

// Write file
await fs.writeFile('file.txt', 'Hello');

// Append to file
await fs.appendFile('file.txt', ' World');
```

## Environment Variables

```javascript
// .env file
PORT=3000
DATABASE_URL=postgresql://...

// Access in code
const port = process.env.PORT || 3000;
```

## Quick Revision

- Node-JS = server-side JavaScript
- Built on V8 engine
- Express.js for web apps
- fs module for file operations
- Environment variables for config

---

## Related Topics

- [[What-is-Node]] - [[What-is-Node|Node.js]] overview
- [[What-is-Express]] - [[What-is-Express|Express.js]]
- [[Create-Server]] - [[Create-Server|Creating servers]]
- [[What-is-FS]] - [[What-is-FS|File system]]
- [[What-is-Path]] - [[What-is-Path|Path module]]
