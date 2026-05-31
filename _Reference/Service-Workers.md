# Service Workers - Background Scripts for Web Apps

## Definition

A Service Worker is a script that runs in the background of a web browser, separate from the web page. It enables features like offline support, push notifications, background sync, and intercepting network requests.

```javascript
// Register service worker
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js')
    .then(registration => console.log('SW registered'))
    .catch(error => console.log('SW registration failed'));
}
```

## Service Worker Lifecycle

```
Register → Install → Activate → Idle
                          ↓
                    Fetch/Message Events
```

### 1. Registration

```javascript
// main.js
if ('serviceWorker' in navigator) {
  window.addEventListener('load', async () => {
    try {
      const registration = await navigator.serviceWorker.register('/sw.js', {
        scope: '/'
      });
      console.log('SW registered:', registration);
    } catch (error) {
      console.log('SW registration failed:', error);
    }
  });
}
```

### 2. Installation

```javascript
// sw.js
const CACHE_NAME = 'v1';
const urlsToCache = [
  '/',
  '/styles/main.css',
  '/scripts/app.js'
];

self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => cache.addAll(urlsToCache))
  );
});
```

### 3. Activation

```javascript
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then(cacheNames => {
      return Promise.all(
        cacheNames
          .filter(name => name !== CACHE_NAME)
          .map(name => caches.delete(name))
      );
    })
  );
});
```

## Common Use Cases

### 1. Offline Support with Caching

```javascript
// sw.js
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then(response => {
        // Return cached version or fetch from network
        return response || fetch(event.request);
      })
  );
});
```

### 2. Cache-First Strategy

```javascript
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then(cached => {
        if (cached) return cached;
        
        return fetch(event.request).then(response => {
          // Clone and cache new responses
          const responseClone = response.clone();
          caches.open(CACHE_NAME)
            .then(cache => cache.put(event.request, responseClone));
          return response;
        });
      })
  );
});
```

### 3. Network-First Strategy

```javascript
self.addEventListener('fetch', (event) => {
  event.respondWith(
    fetch(event.request)
      .then(response => {
        const responseClone = response.clone();
        caches.open(CACHE_NAME)
          .then(cache => cache.put(event.request, responseClone));
        return response;
      })
      .catch(() => caches.match(event.request))
  );
});
```

### 4. Stale-While-Revalidate

```javascript
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.open(CACHE_NAME).then(cache => {
      return cache.match(event.request).then(cachedResponse => {
        const fetchedResponse = fetch(event.request).then(networkResponse => {
          cache.put(event.request, networkResponse.clone());
          return networkResponse;
        });
        
        return cachedResponse || fetchedResponse;
      });
    })
  );
});
```

### 5. Background Sync

```javascript
// sw.js
self.addEventListener('sync', (event) => {
  if (event.tag === 'sync-messages') {
    event.waitUntil(syncMessages());
  }
});

async function syncMessages() {
  const cache = await caches.open('pending-messages');
  const requests = await cache.keys();
  
  for (const request of requests) {
    const response = await cache.match(request);
    const data = await response.json();
    
    await fetch('/api/messages', {
      method: 'POST',
      body: JSON.stringify(data)
    });
    
    await cache.delete(request);
  }
}
```

### 6. Push Notifications

```javascript
// sw.js
self.addEventListener('push', (event) => {
  const data = event.data.json();
  
  event.waitUntil(
    self.registration.showNotification(data.title, {
      body: data.body,
      icon: '/icon.png',
      badge: '/badge.png',
      data: { url: data.url }
    })
  );
});

self.addEventListener('notificationclick', (event) => {
  event.notification.close();
  
  event.waitUntil(
    clients.openWindow(event.notification.data.url)
  );
});
```

### 7. Message Passing

```javascript
// sw.js
self.addEventListener('message', (event) => {
  if (event.data.type === 'CACHE_URLS') {
    event.waitUntil(
      caches.open(CACHE_NAME)
        .then(cache => cache.addAll(event.data.urls))
    );
  }
});

// main.js
navigator.serviceWorker.controller.postMessage({
  type: 'CACHE_URLS',
  urls: ['/page1', '/page2', '/page3']
});
```

## Service Worker API

```javascript
// Check registration
navigator.serviceWorker.ready.then(registration => {
  console.log('SW is active:', registration.active);
});

// Unregister
navigator.serviceWorker.ready.then(registration => {
  registration.unregister();
});

// Get all registrations
navigator.serviceWorker.getRegistrations().then(registrations => {
  registrations.forEach(reg => reg.unregister());
});

// Update service worker
navigator.serviceWorker.ready.then(registration => {
  registration.update();
});
```

## Common Mistakes

```javascript
// ❌ Wrong: Not checking if service worker is supported
navigator.serviceWorker.register('/sw.js');

// ✅ Correct: Always check support first
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js');
}

// ❌ Wrong: Caching too aggressively in install
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => cache.addAll([
        '/',
        '/index.html',
        // ... hundreds of URLs
      ]))
  );
});

// ✅ Correct: Cache incrementally
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then(cached => {
      return cached || fetch(event.request).then(response => {
        return caches.open(CACHE_NAME).then(cache => {
          cache.put(event.request, response.clone());
          return response;
        });
      });
    })
  );
});

// ❌ Wrong: Not handling cache versioning
const CACHE_NAME = 'cache'; // Same name forever

// ✅ Correct: Version your caches
const CACHE_NAME = 'cache-v1';
```

## Cache API

```javascript
// Open a cache
const cache = await caches.open('my-cache');

// Add to cache
await cache.add('/page.html');
await cache.addAll(['/page.html', '/style.css']);

// Put request/response pair
await cache.put(request, response);

// Match
const response = await cache.match(request);

// Delete
await cache.delete(request);

// Get all keys
const keys = await cache.keys();
```

## Quick Revision Summary

- Service Workers run in background, enable offline support
- Lifecycle: Register → Install → Activate → Handle events
- Use Cache API for storing network responses
- Choose strategy: Cache-first, Network-first, Stale-while-revalidate
- Handle messages between main thread and SW
- Always version your caches

## Related Topics

- [[Fetch]] - HTTP requests (intercepted by SW)
- [[Caching]] - Browser caching strategies
- [[OfflineSupport]] - PWA offline capabilities
- [[PushNotifications]] - Web push messaging
- [[WebAppManifest]] - PWA configuration
