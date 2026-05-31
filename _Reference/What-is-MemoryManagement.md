# Memory Management

## Definition

Memory management involves **allocating and freeing memory** for programs.

## How It Works

```javascript
// Allocation
const obj = { name: "John" };
const arr = [1, 2, 3];

// Usage
console.log(obj.name);

// Release (automatic via garbage collection)
obj = null;
arr = null;
```

## Quick Revision

- Memory: allocation → usage → release
- Garbage collection is automatic
- Avoid memory leaks
- Use WeakMap/WeakSet for weak refs

---

## Related Topics

- [[What-is-MemoryManagement]] - [[What-is-MemoryManagement|Memory management]]
- [[What-is-MemoryManagement]] - [[What-is-MemoryManagement|Memory management]]
- [[Memory-Management]] - [[Memory-Management|Memory management]]
- [[What-is-Garbage-Collection]] - [[What-is-Garbage-Collection|Garbage collection]]
