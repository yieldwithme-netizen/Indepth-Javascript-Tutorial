# Performance Measurement

## Definition

Performance measurement involves **tracking and optimizing** application speed.

## Console Timing

```javascript
console.time("loop");
for (let i = 0; i < 1000000; i++) {}
console.timeEnd("loop"); // loop: 2.345ms
```

## Performance API

```javascript
const start = performance.now();
// ... code ...
const end = performance.now();
console.log(`Time: ${end - start}ms`);
```

## Quick Revision

- `console.time()` for timing
- `performance.now()` for precision
- Measure before optimizing

---

## Related Topics

- [[What-is-Optimization]] - [[What-is-Optimization|Optimization]]
- [[Measure-Performance]] - [[Measure-Performance|Measuring performance]]
- [[What-is-Debounce]] - [[What-is-Debounce|Debounce]]
- [[What-is-Throttle]] - [[What-is-Throttle|Throttle]]
