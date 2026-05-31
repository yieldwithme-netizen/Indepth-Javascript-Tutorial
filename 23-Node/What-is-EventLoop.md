# What is the Event Loop in Node.js?

The **Event Loop** is the core mechanism that allows Node.js to perform non-blocking I/O operations despite JavaScript being single-threaded. It continuously checks for and executes asynchronous tasks.

## Definition

The Event Loop is an infinite loop that monitors the call stack and callback queues. When the call stack is empty, it picks callbacks from the queue and pushes them to the stack for execution.

```
┌───────────────────────────┐
│         Callbacks         │
│    (Microtask Queue)      │
└───────────┬───────────────┘
            │
┌───────────▼───────────────┐
│       Event Loop          │
│    (Phases & Queues)      │
└───────────┬───────────────┘
            │
┌───────────▼───────────────┐
│      Call Stack           │
│   (Executing Code)        │
└───────────────────────────┘
```

## How It Works

### Single-Threaded JavaScript

```javascript
// JavaScript is single-threaded - only one thing at a time
console.log('Start');
console.log('Middle');
console.log('End');

// Output:
// Start
// Middle
// End
```

### Asynchronous Operations

```javascript
// Blocking (Synchronous)
console.log('1');
fs.readFileSync('file.txt'); // Blocks until file is read
console.log('2');

// Non-Blocking (Asynchronous)
console.log('1');
fs.readFile('file.txt', () => {
  console.log('2'); // Runs later
});
console.log('3');

// Output:
// 1
// 3
// 2 (after file is read)
```

## Event Loop Phases

```
   ┌───────────────────────────┐
┌─>│         Timers            │ ← setTimeout, setInterval
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     Pending Callbacks     │ ← System operations
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       Idle, Prepare       │ ← Internal use
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │         Poll              │ ← New connections, I/O
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │         Check             │ ← setImmediate
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     Close Callbacks       │ ← socket.on('close')
│  └─────────────┬─────────────┘
└────────────────┘
```

## Types of Asynchronous Operations

### 1. Timers

```javascript
// setTimeout - Execute after delay
console.log('Start');
setTimeout(() => {
  console.log('Timeout'); // Runs after 1000ms
}, 1000);
console.log('End');

// Output:
// Start
// End
// Timeout (after 1 second)
```

### 2. I/O Operations

```javascript
const fs = require('fs');

console.log('Start');

fs.readFile('file.txt', 'utf8', (err, data) => {
  console.log('File read'); // Runs after I/O completes
});

console.log('End');

// Output:
// Start
// End
// File read (when file is ready)
```

### 3. setImmediate vs setTimeout

```javascript
// Order depends on where they're called
setImmediate(() => {
  console.log('Immediate'); // Check phase
});

setTimeout(() => {
  console.log('Timeout'); // Timer phase
}, 0);

// Output can vary, but generally:
// Timeout
// Immediate
```

### 4. Microtasks

```javascript
// Promises (Microtask queue - runs after current phase)
Promise.resolve().then(() => {
  console.log('Promise 1');
});

Promise.resolve().then(() => {
  console.log('Promise 2');
});

// Output:
// Promise 1
// Promise 2
// (runs before next event loop phase)
```

## Practical Examples

### Example 1: Execution Order

```javascript
const fs = require('fs');

console.log('1: Start');

setTimeout(() => {
  console.log('2: Timeout 0ms');
}, 0);

setImmediate(() => {
  console.log('3: Immediate');
});

fs.readFile(__filename, () => {
  console.log('4: File read');

  setTimeout(() => {
    console.log('5: Timeout inside I/O');
  }, 0);

  setImmediate(() => {
    console.log('6: Immediate inside I/O');
  });
});

Promise.resolve().then(() => {
  console.log('7: Promise');
});

console.log('8: End');

// Output:
// 1: Start
// 8: End
// 7: Promise
// 2: Timeout 0ms (or 3: Immediate - order varies)
// 3: Immediate (or 2: Timeout 0ms)
// 4: File read
// 7: Promise (inside I/O callback)
// 6: Immediate inside I/O (or 5: Timeout inside I/O)
// 5: Timeout inside I/O (or 6: Immediate inside I/O)
```

### Example 2: Non-Blocking Server

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  // This callback runs for each request
  // Event loop handles multiple requests concurrently
  console.log('Request received');

  // Non-blocking response
  res.end('Hello');
});

server.listen(3000);

// Multiple clients can connect simultaneously
// Event loop handles them one at a time without blocking
```

### Example 3: Preventing Blocking

```javascript
// BAD: Blocking the event loop
function heavyComputation() {
  let sum = 0;
  for (let i = 0; i < 1e9; i++) {
    sum += i;
  }
  return sum;
}

// GOOD: Use worker threads for CPU-intensive tasks
const { Worker } = require('worker_threads');

function runHeavyTask() {
  return new Promise((resolve, reject) => {
    const worker = new Worker('./heavy-task.js');
    worker.on('message', resolve);
    worker.on('error', reject);
  });
}
```

## Process.nextTick vs setImmediate

```javascript
// nextTick runs before setImmediate
process.nextTick(() => {
  console.log('nextTick');
});

setImmediate(() => {
  console.log('setImmediate');
});

// Output:
// nextTick
// setImmediate
```

## Monitoring Event Loop Lag

```javascript
const { monitorEventLoopDelay } = require('perf_hooks');

const histogram = monitorEventLoopDelay({ resolution: 20 });
histogram.enable();

// Log every 5 seconds
setInterval(() => {
  console.log(`Event loop lag: ${histogram.mean / 1e6}ms`);
  histogram.reset();
}, 5000);
```

## Common Use Cases

- Handling thousands of concurrent connections
- File I/O operations
- Network requests
- Timer-based operations
- Database queries
- Real-time applications
- Background job processing

## Common Mistakes

### 1. Blocking the Event Loop
```javascript
// Bad - Blocks everything
app.get('/compute', (req, res) => {
  const result = computeExpensiveOperation(); // 10 seconds
  res.send(result);
});

// Good - Offload heavy work
app.get('/compute', (req, res) => {
  worker.postMessage(data); // Send to worker thread
  res.send('Processing...');
});
```

### 2. Misunderstanding setTimeout Delay
```javascript
// 0ms doesn't mean "run immediately"
setTimeout(() => {
  console.log('Runs later, not now');
}, 0);
```

### 3. Callback Hell (Old Pattern)
```javascript
// Bad - Deeply nested callbacks
getData((a) => {
  getMoreData(a, (b) => {
    getEvenMoreData(b, (c) => {
      console.log(c);
    });
  });
});

// Good - Use Promises or async/await
async function getDataSequentially() {
  const a = await getData();
  const b = await getMoreData(a);
  const c = await getEvenMoreData(b);
  console.log(c);
}
```

## Quick Revision

| Concept | Description |
|---------|-------------|
| Call Stack | Where code executes |
| Callback Queue | Waiting callbacks (FIFO) |
| Microtask Queue | Promises, process.nextTick |
| Timers Phase | setTimeout, setInterval |
| Poll Phase | I/O callbacks |
| Check Phase | setImmediate |
| Close Phase | Cleanup callbacks |

| Method | When It Runs |
|--------|--------------|
| `process.nextTick` | Before next event loop phase |
| `Promise.then` | After current operation, before next phase |
| `setTimeout(fn, 0)` | Next timer phase |
| `setImmediate` | Next check phase |

## Related Topics

- [[What-is-Node]] - Node.js fundamentals
- [[What-is-HTTP]] - Creating servers with event loop
- [[Create-Server]] - HTTP server handling multiple requests
- [[What-is-Streams]] - Stream-based processing
- [[What-is-FS]] - Async file operations

---

**Key Takeaway:** The Event Loop enables Node.js to handle thousands of concurrent operations efficiently. Always avoid blocking it with synchronous heavy operations - use asynchronous methods, worker threads, or child processes for CPU-intensive tasks.
