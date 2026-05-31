# What-is-SPA

## Definition

SPA (Single Page Application) loads **one HTML page** and dynamically updates content.

## How It Works

```javascript
const routes = {
    '/': '<h1>Home</h1>',
    '/about': '<h1>About</h1>'
};

function navigate(path) {
    document.getElementById('app').innerHTML = routes[path];
}
```

## Quick Revision

- SPA = one HTML page
- Dynamic content updates
- Client-side routing
- Faster transitions

---

## Related Topics

- [[What-is-SPA]] - [[What-is-SPA|SPA]]
- [[What-is-SPA]] - [[What-is-SPA|SPA]]
- [[SPA Architecture]] - [[SPA Architecture|SPA architecture]]
- [[What-is-Routing]] - [[What-is-Routing|Routing]]
