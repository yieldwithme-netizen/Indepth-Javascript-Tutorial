# Offline Support

## Definition

Offline support lets apps **work without internet**.

## Implementation

```javascript
// Service Worker
self.addEventListener('fetch', (event) => {
    event.respondWith(
        caches.match(event.request).then((response) => {
            return response || fetch(event.request);
        })
    );
});
```

## Quick Revision

- Offline = work without internet
- Service workers enable offline
- Cache assets
- IndexedDB for data

---

## Related Topics

- [[What-is-ServiceWorkers]] - [[What-is-ServiceWorkers|Service workers]]
- [[OfflineSupport]] - [[OfflineSupport|Offline support]]
- [[ServiceWorkers]] - [[ServiceWorkers|Service workers]]
- [[CacheAPI]] - [[CacheAPI|CacheAPI]]
