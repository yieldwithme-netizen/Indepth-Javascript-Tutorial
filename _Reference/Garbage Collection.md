# Garbage Collection

## Definition

Garbage collection **automatically frees unused memory**.

## How It Works

```javascript
let obj = { name: "John" };
obj = null; // object becomes unreachable, gets collected
```

## Memory Leaks

```javascript
// ❌ Global variable
function leak() {
    leakedVar = "I leak!";
}

// ✅ Fix: use local variables
function noLeak() {
    const localVar = "I don't leak";
}
```

## Quick Revision

- Automatic memory management
- Collects unreachable objects
- Avoid memory leaks
- Use WeakMap/WeakSet for weak refs

---

## Related Topics

- [[What-is-Garbage-Collection]] - [[What-is-Garbage-Collection|Garbage collection]]
- [[Garbage-Collection]] - [[Garbage Collection|Garbage collection]]
- [[Memory-Management]] - [[Memory-Management|Memory management]]
- [[What-is-MemoryLeak]] - [[What-is-MemoryLeak|Memory leaks]]
