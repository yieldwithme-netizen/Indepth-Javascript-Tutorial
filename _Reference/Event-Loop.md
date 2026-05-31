# Event Loop

## Definition

The event loop **handles asynchronous operations** in JavaScript.

## How It Works

```javascript
console.log("1");              // Sync

setTimeout(() => {
    console.log("2");          // Macrotask
}, 0);

Promise.resolve().then(() => {
    console.log("3");          // Microtask
});

console.log("4");              // Sync

// Output: 1, 4, 3, 2
```

## Quick Revision

- Event loop handles async operations
- Call stack executes sync code
- Microtasks (Promises) before macrotasks (setTimeout)
- Output: 1, 4, 3, 2

---

## Related Topics

- [[What-is-EventLoop]] - [[What-is-EventLoop|Event loop]]
- [[Event-Loop]] - [[Event-Loop|Event loop]]
- [[Event Loop]] - [[Event Loop|Event loop]]
- [[What-is-Async]] - [[What-is-Async|Async]]
- [[What-is-Promise]] - [[What-is-Promise|Promises]]
