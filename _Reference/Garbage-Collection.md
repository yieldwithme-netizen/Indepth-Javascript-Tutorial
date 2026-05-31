# Garbage Collection

## Definition

Garbage collection is the **automatic memory management** process that frees unused memory.

## How It Works

```javascript
// Memory allocated
let obj = { name: "John" };

// Memory freed (when obj is no longer referenced)
obj = null;
```

## Mark and Sweep

1. Mark all reachable objects
2. Sweep (remove) unmarked objects

```javascript
function createObjects() {
    let a = { name: "a" };
    let b = { name: "b" };
    // Both reachable
    return a;
    // b becomes unreachable, gets garbage collected
}
```

## Memory Leaks

```javascript
// ❌ Memory leak: global variable
function leak() {
    leakedVar = "I leak!";
}

// ❌ Memory leak: forgotten timer
setInterval(() => {
    // code
}, 1000);

// ✅ Fix: clear timer
const id = setInterval(() => {
    // code
}, 1000);
clearInterval(id);
```

## Quick Revision

- Garbage collection = automatic memory management
- Mark and sweep algorithm
- Unreachable objects get collected
- Avoid memory leaks
- Use WeakMap/WeakSet for weak references

---

## Related Topics

- [[What-is-Garbage-Collection]] - [[What-is-Garbage-Collection|Garbage collection]]
- [[What-is-MemoryLeak]] - [[What-is-MemoryLeak|Memory leaks]]
- [[Prevent-MemoryLeaks]] - [[Prevent-MemoryLeaks|Preventing leaks]]
- [[Memory-Management]] - [[Memory-Management|Memory management]]
