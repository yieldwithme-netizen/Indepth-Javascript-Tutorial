# What-is-CacheAPI

## Definition

CacheAPI stores **network responses** for offline use.

## Example

```javascript
const cache = await caches.open('v1');
await cache.add('/index.html');
const response = await cache.match('/index.html');
```

## Quick Revision

- CacheAPI for offline support
- Open cache with caches.open()
- Add/match responses
- Use with service workers

---

## Related Topics

- [[What-is-CacheAPI]] - [[What-is-CacheAPI|CacheAPI]]
- [[What-is-CacheAPI]] - [[What-is-CacheAPI|CacheAPI]]
- [[CacheAPI]] - [[CacheAPI|CacheAPI]]
- [[What-is-ServiceWorkers]] - [[What-is-ServiceWorkers|Service workers]]
