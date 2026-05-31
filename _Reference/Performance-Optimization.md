# Performance Optimization

## Definition

Performance optimization **improves application speed**.

## Techniques

```javascript
// Lazy loading
const LazyComponent = React.lazy(() => import('./Component'));

// Memoization
const MemoizedComponent = React.memo(Component);

// Debounce
function debounce(fn, delay) {
    let timer;
    return function(...args) {
        clearTimeout(timer);
        timer = setTimeout(() => fn(...args), delay);
    };
}
```

## Quick Revision

- Lazy load components
- Memoize expensive calculations
- Debounce/throttle events
- Optimize images

---

## Related Topics

- [[What-is-Optimization]] - [[What-is-Optimization|Optimization]]
- [[Performance-Optimization]] - [[Performance-Optimization|Performance optimization]]
- [[What-is-LazyLoading]] - [[What-is-LazyLoading|Lazy loading]]
- [[What-is-Debounce]] - [[What-is-Debounce|Debounce]]
