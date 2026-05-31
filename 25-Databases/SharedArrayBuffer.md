# SharedArrayBuffer

## Definition

SharedArrayBuffer shares **memory between workers**.

## Example

```javascript
// Main thread
const buffer = new SharedArrayBuffer(1024);
const arr = new Int32Array(buffer);
arr[0] = 42;

// Worker
const arr = new Int32Array(buffer);
console.log(arr[0]); // 42
```

## Quick Revision

- SharedArrayBuffer for shared memory
- Use with Web Workers
- Requires cross-origin isolation
- Use Atomics for thread-safe ops

---

## Related Topics

- [[Atomics]] - [[Atomics|Atomics]]
- [[SharedArrayBuffer]] - [[SharedArrayBuffer|SharedArrayBuffer]]
- [[Web-Workers]] - [[Web-Workers|Web Workers]]
