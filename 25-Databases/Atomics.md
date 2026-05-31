# Atomics

## Definition

Atomics provides **atomic operations** on shared memory.

## Example

```javascript
const buffer = new SharedArrayBuffer(1024);
const arr = new Int32Array(buffer);

Atomics.add(arr, 0, 5);  // Add 5
Atomics.sub(arr, 0, 2);  // Subtract 2
Atomics.load(arr, 0);    // Read value
```

## Quick Revision

- Atomics for thread-safe operations
- Works with SharedArrayBuffer
- Use for: Web Workers shared memory

---

## Related Topics

- [[SharedArrayBuffer]] - [[SharedArrayBuffer|SharedArrayBuffer]]
- [[Atomics]] - [[Atomics|Atomics]]
