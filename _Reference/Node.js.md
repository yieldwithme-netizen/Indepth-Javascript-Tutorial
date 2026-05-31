# Node.js

## Definition

Node.js is a **JavaScript runtime** that lets you run JavaScript **outside the browser** on servers, desktops, and embedded devices.

## Key Features

- **V8 Engine** - Google's fast JavaScript engine
- **Event-Driven** - Non-blocking I/O
- **npm** - Package manager with 1M+ packages
- **Cross-Platform** - Windows, macOS, Linux

## Installing Node.js

```bash
# Download from nodejs.org
# Or use nvm (recommended)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install --lts
nvm use --lts

# Verify installation
node --version
npm --version
```

## Running JavaScript

```bash
# Run a file
node app.js

# Interactive mode (REPL)
node

# Run with watch mode
node --watch app.js
```

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

## Quick Revision

- Node.js = server-side JavaScript
- Uses V8 engine
- Event-driven, non-blocking
- npm for packages
- Core modules: fs, path, http, events

---

## Related Topics

- [[What-is-Node]] - [[What-is-Node|Node.js]] overview
- [[Install-NodeJS]] - [[Install-NodeJS|Installation]]
- [[What-is-NPM]] - [[What-is-NPM|NPM]]
- [[Create-Server]] - [[Create-Server|HTTP server]]
- [[What-is-EventLoop]] - [[What-is-EventLoop|Event loop]]
