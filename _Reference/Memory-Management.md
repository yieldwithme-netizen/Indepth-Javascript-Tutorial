# Memory Management

## Definition

Memory management involves **allocating and freeing memory** for JavaScript programs.

## Memory Lifecycle

```javascript
// 1. Allocation
const obj = { name: "John" };
const arr = [1, 2, 3];
const str = "Hello";

// 2. Usage
console.log(obj.name);
console.log(arr.length);

// 3. Release (automatic via garbage collection)
obj = null;
arr = null;
str = null;
```

## Memory Leaks

```javascript
// ❌ Global variable
function leak() {
    leakedVar = "I leak!";
}

// ❌ Forgotten timer
setInterval(() => {}, 1000);

// ❌ Closures
function createClosure() {
    const bigData = new Array(1000000).fill('x');
    return function() {
        return bigData;
    };
}

// ✅ Fix: clear timer, release references
const id = setInterval(() => {}, 1000);
clearInterval(id);
```

## Quick Revision

- Memory: allocation → usage → release
- Garbage collection is automatic
- Avoid memory leaks
- Release references when done
- Use WeakMap/WeakSet for weak refs

---

## Related Topics

- [[What-is-MemoryManagement]] - [[What-is-MemoryManagement|Memory management]]
- [[Memory-Management]] - [[Memory-Management|Memory management]]
- [[What-is-Garbage-Collection]] - [[What-is-Garbage-Collection|Garbage collection]]
- [[What-is-MemoryLeak]] - [[What-is-MemoryLeak|Memory leaks]]
