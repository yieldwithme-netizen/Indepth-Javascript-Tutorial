# What is a JavaScript Runtime?

## Definition

A [[JavaScript]] runtime is the **environment** that executes your JavaScript code. It provides everything JS needs to run: the engine, APIs, and [[event]] loop.

## Components of a JS Runtime

```
┌─────────────────────────────────────────┐
│           JavaScript Runtime            │
├─────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────────┐  │
│  │ JS Engine   │  │ Web APIs        │  │
│  │ (V8, etc.)  │  │ (setTimeout,    │  │
│  │             │  │  fetch, DOM)    │  │
│  └─────────────┘  └─────────────────┘  │
│  ┌─────────────┐  ┌─────────────────┐  │
│  │ Callback    │  │ Event Loop      │  │
│  │ Queue       │  │                 │  │
│  └─────────────┘  └─────────────────┘  │
└─────────────────────────────────────────┘
```

## Browser vs Node.js Runtime

| Component | Browser | Node.js |
|-----------|---------|---------|
| JS Engine | V8, SpiderMonkey, etc. | V8 |
| [[DOM]] API | ✅ | ❌ |
| [[window]] | ✅ | ❌ |
| File System | ❌ | ✅ (fs module) |
| Network | fetch, XMLHttpRequest | http, https |
| Process | ❌ | ✅ |

## The [[event]] Loop

```javascript
// Synchronous code (runs immediately)
console.log("1");

// Asynchronous code (goes to callback queue)
setTimeout(() => console.log("2"), 0);

// Promise (goes to microtask queue)
Promise.resolve().then(() => console.log("3"));

console.log("4");

// Output: 1, 4, 3, 2
```

### Why This Order?

```
1. Call stack: console.log("1") → prints "1"
2. Call stack: setTimeout() → registers callback, moves on
3. Call stack: Promise.resolve() → registers .then, moves on
4. Call stack: console.log("4") → prints "4"
5. Microtask queue: Promise callback → prints "3"
6. Callback queue: setTimeout callback → prints "2"
```

## Runtime APIs

### Browser APIs
```javascript
// DOM
document.querySelector();

// Storage
localStorage.setItem();
sessionStorage.setItem();

// Network
fetch();
XMLHttpRequest();

// Timers
setTimeout();
setInterval();

// Geolocation
navigator.geolocation.getCurrentPosition();
```

### Node.js APIs
```javascript
// File System
const fs = require('fs');
fs.readFile();

// HTTP
const http = require('http');
http.createServer();

// Path
const path = require('path');
path.join();

// Process
process.env;
process.argv;
```

## Quick Revision

- JS Runtime = environment that executes [[JavaScript]]
- Components: Engine + APIs + Event Loop + Callback Queue
- Browser has [[DOM]], Node.js has file system
- Event loop handles [[async]] code
- Microtasks ([[Promise]]s) run before macrotasks (setTimeout)

---

## Related Topics

- [[What-is-JavaScript]] - JS overview
- [[What-is-Browser-Engine]] - Browser engines
- [[What-is-V8-Engine]] - V8 deep dive
- [[What-is-Node]] - Node.js runtime