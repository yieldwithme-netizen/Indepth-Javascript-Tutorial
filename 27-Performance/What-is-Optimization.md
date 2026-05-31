# What is Performance Optimization?

## Definition

Performance optimization is **making your code run faster** and use fewer resources.

## Key Metrics

| Metric | Description | Target |
|--------|-------------|--------|
| FCP | First Contentful Paint | < 1.8s |
| LCP | Largest Contentful Paint | < 2.5s |
| FID | First Input Delay | < 100ms |
| CLS | Cumulative Layout Shift | < 0.1 |

## Common Techniques

### 1. Lazy Loading

```javascript
// Images
<img loading="lazy" src="image.jpg">

// Components
const LazyComponent = React.lazy(() => import('./Component'));
```

### 2. Code Splitting

```javascript
// Dynamic import
button.addEventListener('click', async () => {
    const module = await import('./heavyModule');
    module.doSomething();
});
```

### 3. Memoization

```javascript
// React
const MemoizedComponent = React.memo(Component);

// Vanilla JS
function memoize(fn) {
    const cache = {};
    return function(...args) {
        const key = JSON.stringify(args);
        return cache[key] || (cache[key] = fn(...args));
    };
}
```

### 4. Debounce

```javascript
function debounce(fn, delay) {
    let timer;
    return function(...args) {
        clearTimeout(timer);
        timer = setTimeout(() => fn(...args), delay);
    };
}

input.addEventListener('input', debounce(search, 300));
```

## Quick Revision

- Optimize: FCP, LCP, FID, CLS
- Lazy load: images, components
- Code split: dynamic imports
- Memoize: cache results
- Debounce: limit function calls

---

## Related Topics

- [[What-is-Optimization]] - Optimization overview
- [[What-is-LazyLoading]] - Lazy loading
- [[What-is-CodeSplitting]] - Code splitting
- [[What-is-TreeShaking]] - Tree shaking
- [[What-is-MemoryLeak]] - Memory leaks
- [[What-is-Debounce]] - Debounce
- [[What-is-Throttle]] - Throttle
