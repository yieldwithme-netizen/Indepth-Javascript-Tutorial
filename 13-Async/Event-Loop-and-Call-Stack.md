# Event Loop and Call Stack

## Definition

Event loop and call stack **manage code execution** in JavaScript.

## How It Works

```javascript
console.log("1");        // Call stack
setTimeout(() => {}, 0); // Web API
Promise.resolve().then(); // Microtask queue
console.log("4");        // Call stack
```

## Quick Revision

- Call stack: executes synchronous code
- Web APIs: handle async operations
- Callback queue: stores callbacks
- Microtask queue: stores Promises
- Event loop: orchestrates execution

---

## Related Topics

- [[What-is-EventLoop]] - [[What-is-EventLoop|Event loop]]
- [[Event Loop]] - [[Event Loop|Event loop]]
- [[Event-Loop-and-Call-Stack]] - [[Event-Loop-and-Call-Stack|Event loop and call stack]]
- [[What-is-Async]] - [[What-is-Async|Async]]
