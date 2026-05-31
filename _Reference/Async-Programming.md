# Async Programming

## Definition

Async programming handles **operations that take time** without blocking the main thread.

## Patterns

```javascript
// Callbacks
getData(function(result) {
    console.log(result);
});

// Promises
getData()
    .then(result => console.log(result));

// Async/Await
const result = await getData();
console.log(result);
```

## Quick Revision

- Async: non-blocking operations
- Callbacks: old pattern
- Promises: better pattern
- Async/await: modern pattern

---

## Related Topics

- [[What-is-Async]] - [[What-is-Async|Async]]
- [[Async-Programming]] - [[Async-Programming|Async programming]]
- [[What-is-Promise]] - [[What-is-Promise|Promises]]
- [[What-is-AsyncAwait]] - [[What-is-AsyncAwait|Async/await]]
- [[What-is-Callback]] - [[What-is-Callback|Callbacks]]
