# Caching

## Definition

Caching **stores data** for faster future access.

## Browser Caching

```javascript
// Cache API
const cache = await caches.open('v1');
await cache.add('/data.json');
const response = await cache.match('/data.json');
```

## Memory Caching

```javascript
const cache = new Map();

function getData(key) {
    if (cache.has(key)) {
        return cache.get(key);
    }
    const data = fetchData(key);
    cache.set(key, data);
    return data;
}
```

## Quick Revision

- Caching = store data for reuse
- Browser: Cache API
- Memory: Map/Object
- Reduces load times

---

## Related Topics

- [[What-is-Caching]] - [[What-is-Caching|Caching]]
- [[Caching]] - [[Caching|Caching]]
- [[CacheAPI]] - [[CacheAPI|CacheAPI]]
- [[What-is-LocalStorage]] - [[What-is-LocalStorage|LocalStorage]]
