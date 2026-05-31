# What-is-AbortController

## Definition

AbortController **cancels fetch requests** and other async operations.

## Basic Usage

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

- AbortController cancels fetch
- `controller.abort()` to cancel
- Check `AbortError` in catch
- Use for cleanup, timeout

---

## Related Topics

- [[CancellationPatterns]] - [[CancellationPatterns|Cancellation patterns]]
- [[What-is-Fetch]] - [[What-is-Fetch|Fetch]]
- [[React-UseEffect-Cleanup]] - [[React-UseEffect-Cleanup|useEffect cleanup]]
