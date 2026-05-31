# Event Loop

## Definition

The event loop is the **mechanism that handles asynchronous operations** in JavaScript.

## How It Works

```
┌───────────────────────────┐
│        Call Stack          │
│  (executes code)          │
└─────────┬─────────────────┘
          │
          ▼
┌───────────────────────────┐
│     Web APIs / Node APIs  │
│  (setTimeout, fetch, etc) │
└─────────┬─────────────────┘
          │
          ▼
┌───────────────────────────┐
│      Callback Queue       │
│  (setTimeout callbacks)   │
└─────────┬─────────────────┘
          │
          ▼
┌───────────────────────────┐
│    Microtask Queue        │
│  (Promises, queueMicrotask)│
└─────────┬─────────────────┘
          │
          ▼
┌───────────────────────────┐
│       Event Loop          │
│  (checks queues, pushes  │
│   to call stack)          │
└───────────────────────────┘
```

## Example

```javascript
console.log("1");              // Sync - Call Stack

setTimeout(() => {
    console.log("2");          // Macrotask - Callback Queue
}, 0);

Promise.resolve().then(() => {
    console.log("3");          // Microtask - Microtask Queue
});

console.log("4");              // Sync - Call Stack

// Output: 1, 4, 3, 2
```

## Quick Revision

- Event loop handles async operations
- Call stack executes synchronous code
- Web APIs handle async operations
- Microtasks (Promises) run before macrotasks (setTimeout)
- Output: 1, 4, 3, 2

---

## Related Topics

- [[What-is-EventLoop]] - [[What-is-EventLoop|Event loop]]
- [[Event Loop]] - [[Event Loop|Event loop]]
- [[What-is-Async]] - [[What-is-Async|Async]]
- [[What-is-Promise]] - [[What-is-Promise|Promises]]
- [[What-is-SetTimeout]] - [[What-is-SetTimeout|setTimeout]]
