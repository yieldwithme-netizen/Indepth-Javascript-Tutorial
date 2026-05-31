# Service Workers

## Definition

Service workers are **background scripts** that enable offline support and push notifications.

## Basic Usage

```javascript
// Register
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/sw.js');
}

// sw.js
self.addEventListener('install', (event) => {
    event.waitUntil(
        caches.open('v1').then((cache) => {
            return cache.addAll([
                '/',
                '/index.html',
                '/style.css',
                '/app.js'
            ]);
        })
    );
});

self.addEventListener('fetch', (event) => {
    event.respondWith(
        caches.match(event.request).then((response) => {
            return response || fetch(event.request);
        })
    );
});
```

## Quick Revision

- Service workers = background scripts
- Enable offline support
- Cache assets
- Handle push notifications
- Register with `navigator.serviceWorker`

---

## Related Topics

- [[What-is-ServiceWorkers]] - [[What-is-ServiceWorkers|Service workers]]
- [[ServiceWorkers]] - [[ServiceWorkers|Service workers]]
- [[OfflineSupport]] - [[OfflineSupport|Offline support]]
- [[PushNotifications]] - [[PushNotifications|Push notifications]]
