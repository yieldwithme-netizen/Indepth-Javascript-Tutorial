# Memory Leaks

## Definition

Memory leaks occur when **memory is not freed** after use.

## Common Causes

```javascript
// Global variables
function leak() {
    leakedVar = "I leak!";
}

// Forgotten timers
setInterval(() => {}, 1000);

// Closures
function createClosure() {
    const data = new Array(1000000).fill('x');
    return function() { return data; };
}
```

## Quick Revision

- Memory leak: unused memory not freed
- Avoid global variables
- Clear timers when done
- Be careful with closures

---

## Related Topics

- [[What-is-MemoryLeak]] - [[What-is-MemoryLeak|Memory leaks]]
- [[Memory-Leaks]] - [[Memory-Leaks|Memory leaks]]
- [[Prevent-MemoryLeaks]] - [[Prevent-MemoryLeaks|Preventing memory leaks]]
- [[Memory-Management]] - [[Memory-Management|Memory management]]
