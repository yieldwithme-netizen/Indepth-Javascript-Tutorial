# Cancellation Patterns

## Definition

Cancellation patterns handle **aborting async operations**.

## AbortController

```javascript
const controller = new AbortController();

fetch('/api/data', { signal: controller.signal })
    .then(response => response.json())
    .catch(err => {
        if (err.name === 'AbortError') {
            console.log('Request cancelled');
        }
    });

// Cancel
controller.abort();
```

## Quick Revision

- AbortController for fetch cancellation
- `controller.abort()` to cancel
- Check `AbortError` in catch
- Use for: cleanup, timeout

---

## Related Topics

- [[What-is-AbortController]] - [[What-is-AbortController|AbortController]]
- [[CancellationPatterns]] - [[CancellationPatterns|Cancellation patterns]]
- [[What-is-Fetch]] - [[What-is-Fetch|Fetch]]
- [[React-UseEffect-Cleanup]] - [[React-UseEffect-Cleanup|useEffect cleanup]]
