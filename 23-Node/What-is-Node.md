# What is Node.js?

## Definition

Node.js is a **JavaScript runtime** that lets you run JavaScript outside the browser.

## Core Modules

```javascript
// File System
const fs = require('fs');
fs.readFileSync('file.txt', 'utf8');

// Path
const path = require('path');
path.join(__dirname, 'file.txt');

// HTTP
const http = require('http');
http.createServer((req, res) => {
    res.end('Hello World');
}).listen(3000);

// Events
const EventEmitter = require('events');
const emitter = new EventEmitter();
```

## Event Loop

```
   ┌───────────────────────────┐
┌─>│           timers          │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │
│  └─────────────┬─────────────┘      ┌───────────────┐
│  ┌─────────────┴─────────────┐      │   incoming:   │
│  │           poll            │<─────┤  connections, │
│  └─────────────┬─────────────┘      │   data, etc.  │
│  ┌─────────────┴─────────────┐      └───────────────┘
│  │           check           │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │
   └───────────────────────────┘
```

## Quick Revision

- Node.js = JavaScript runtime (V8 engine)
- Core modules: fs, path, http, events
- Event loop handles async operations
- npm for package management
- Use for: servers, APIs, tools

---

## Related Topics

- [[What-is-Node]] - Node.js overview
- [[What-is-NodeModules]] - Node modules
- [[Read-Files]] - File reading
- [[Write-Files]] - File writing
- [[Create-Server]] - HTTP server
- [[What-is-EventLoop]] - Event loop
