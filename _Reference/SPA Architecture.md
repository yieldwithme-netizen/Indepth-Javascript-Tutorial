# SPA Architecture

## Definition

SPA (Single Page Application) loads **one HTML page** and dynamically updates content.

## How It Works

```javascript
// Client-side routing
const routes = {
    '/': '<h1>Home</h1>',
    '/about': '<h1>About</h1>',
    '/contact': '<h1>Contact</h1>'
};

function navigate(path) {
    document.getElementById('app').innerHTML = routes[path];
}

window.addEventListener('popstate', () => {
    navigate(window.location.pathname);
});
```

## Quick Revision

- SPA = one HTML page
- Dynamic content updates
- Client-side routing
- Faster page transitions
- Use frameworks (React, Vue, Angular)

---

## Related Topics

- [[What-is-SPA]] - [[What-is-SPA|SPA]]
- [[SPA Architecture]] - [[SPA Architecture|SPA architecture]]
- [[What-is-Routing]] - [[What-is-Routing|Routing]]
- [[What-is-VirtualDOM]] - [[What-is-VirtualDOM|Virtual DOM]]
