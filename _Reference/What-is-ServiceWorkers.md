# What-is-ServiceWorkers

## Definition

Service workers are **background scripts** for offline support.

## Example

```javascript
// Register
navigator.serviceWorker.register('/sw.js');

// sw.js
self.addEventListener('install', (event) => {
    event.waitUntil(
        caches.open('v1').then((cache) => {
            return cache.addAll(['/index.html', '/style.css']);
        })
    );
});
```

## Quick Revision

- Service workers = background scripts
- Enable offline support
- Cache assets
- Handle push notifications

---

## Related Topics

- [[What-is-ServiceWorkers]] - [[What-is-ServiceWorkers|Service workers]]
- [[What-is-ServiceWorkers]] - [[What-is-ServiceWorkers|Service workers]]
- [[ServiceWorkers]] - [[ServiceWorkers|Service workers]]
- [[OfflineSupport]] - [[OfflineSupport|Offline support]]
