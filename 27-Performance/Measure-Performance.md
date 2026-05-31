# How to Measure Performance

## Definition

Performance measurement involves tracking execution time, memory usage, and resource consumption to identify bottlenecks and optimize your code.

## Timing Functions

### `console.time()` / `console.timeEnd()`

```javascript
console.time("loop");
for (let i = 0; i < 1_000_000; i++) {
  // do work
}
console.timeEnd("loop"); // loop: 12.345ms
```

### `performance.now()`

```javascript
const start = performance.now();
// ... operation
const end = performance.now();
console.log(`Duration: ${end - start} ms`);
```

### `Date.now()`

```javascript
const start = Date.now();
// ... operation
const duration = Date.now() - start;
console.log(`Duration: ${duration} ms`);
```

## Browser Performance APIs

### Performance Observer

```javascript
const observer = new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    console.log(entry.name, entry.duration);
  }
});

observer.observe({ type: "measure", buffered: true });

// Usage
performance.mark("start");
// ... operation
performance.mark("end");
performance.measure("myOperation", "start", "end");
```

### Navigation Timing API

```javascript
window.addEventListener("load", () => {
  const timing = performance.getEntriesByType("navigation")[0];
  console.log(`DOM Loaded: ${timing.domContentLoadedEventEnd} ms`);
  console.log(`Full Load: ${timing.loadEventEnd} ms`);
});
```

### Resource Timing API

```javascript
const resources = performance.getEntriesByType("resource");
resources.forEach((r) => {
  console.log(`${r.name}: ${r.duration.toFixed(2)} ms`);
});
```

## Memory Profiling

```javascript
// Check memory usage (Chrome DevTools)
if (performance.memory) {
  console.log(`Used: ${performance.memory.usedJSHeapSize / 1e6} MB`);
  console.log(`Total: ${performance.memory.totalJSHeapSize / 1e6} MB`);
}
```

## Benchmarking

```javascript
function benchmark(fn, iterations = 1000) {
  const start = performance.now();
  for (let i = 0; i < iterations; i++) {
    fn();
  }
  return performance.now() - start;
}

const time1 = benchmark(() => Array.from({ length: 100 }, (_, i) => i));
const time2 = benchmark(() => [...Array(100)].map((_, i) => i));
console.log(`Method 1: ${time1.toFixed(2)} ms`);
console.log(`Method 2: ${time2.toFixed(2)} ms`);
```

## Common Use Cases

- **Identify slow code**: Profile execution time
- **Optimize loops**: Compare iteration approaches
- **Measure render time**: Track UI responsiveness
- **Memory leak detection**: Monitor heap growth

## Common Mistakes

```javascript
// Mistake: Measuring in release mode only
// Always test in development AND production environments

// Mistake: Single measurement
// Always run multiple iterations and average
const avgTime = Array.from({ length: 10 }, () => {
  const start = performance.now();
  // operation
  return performance.now() - start;
}).reduce((a, b) => a + b) / 10;
```

## Related Topics

- [[Optimize-Rendering]]
- [[What-is-CodeSplitting]]
- [[What-is-LazyLoading]]
- [[What-is-TreeShaking]]

## Quick Revision

| Method | Precision | Use Case |
|--------|-----------|----------|
| `console.time` | Low | Quick debugging |
| `performance.now` | High (μs) | Accurate measurement |
| `Date.now` | Low (ms) | Simple timing |
| `PerformanceObserver` | High | Browser metrics |
