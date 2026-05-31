# What is Memoization

Memoization is an optimization technique that stores the results of expensive function calls and returns the cached result when the same inputs occur again.

## Definition

Memoization caches function results based on input arguments, avoiding redundant computations for previously seen inputs.

```javascript
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// Example
const expensiveSquare = memoize(n => {
  console.log("Computing...");
  return n * n;
});

expensiveSquare(4); // "Computing..." -> 16
expensiveSquare(4); // 16 (cached, no log)
```

## Why Memoization Matters

- Reduces redundant calculations
- Speeds up recursive algorithms
- Improves performance for pure functions
- Essential for dynamic programming

## Practical Examples

### Fibonacci with Memoization
```javascript
const fibonacci = memoize(n => {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
});

fibonacci(40); // Fast! Without memoization: very slow
```

### Factorial
```javascript
const factorial = memoize(n => {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
});

factorial(10); // 3628800 (cached after first call)
```

### API Response Caching
```javascript
const memoizeFetch = memoize(url =>
  fetch(url).then(r => r.json())
);

// First call: network request
await memoizeFetch("/api/users");
// Second call: returns cached promise
await memoizeFetch("/api/users");
```

### Expensive Computations
```javascript
const heavyCalculation = memoize(data => {
  return data
    .filter(item => item.active)
    .map(item => ({ ...item, score: item.value * item.weight }))
    .sort((a, b) => b.score - a.score);
});
```

## Cache Management

```javascript
function memoizeWithLimit(fn, limit = 100) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn.apply(this, args);
    if (cache.size >= limit) {
      const firstKey = cache.keys().next().value;
      cache.delete(firstKey);
    }
    cache.set(key, result);
    return result;
  };
}
```

## Common Mistakes

- Memoizing functions with side effects
- Using memoization on frequently changing inputs
- Cache key that doesn't capture all relevant inputs
- Not considering memory usage for large caches

## Quick Revision

- Memoization caches results based on function inputs
- Great for recursive algorithms and expensive computations
- Only use on pure functions (same input = same output)
- Consider cache size limits for long-running applications

## Related Topics

- [[What-is-Currying]]
- [[What-is-Composition]]
- [[What-is-Immutability]]
- [[Performance-Optimization]]
