# Page Load Events

## Definition

Page load events fire at different stages of the **browser loading process**.

## Events Order

```javascript
// 1. DOMContentLoaded - HTML parsed, DOM ready
document.addEventListener('DOMContentLoaded', () => {
    console.log('DOM ready');
});

// 2. load - All resources loaded (images, CSS, etc)
window.addEventListener('load', () => {
    console.log('Page fully loaded');
});

// 3. beforeunload - User leaving page
window.addEventListener('beforeunload', (e) => {
    e.preventDefault();
    e.returnValue = '';
});

// 4. unload - Page being unloaded
window.addEventListener('unload', () => {
    console.log('Page unloaded');
});
```

## DOMContentLoaded vs load

```javascript
// DOMContentLoaded: faster, DOM ready
document.addEventListener('DOMContentLoaded', () => {
    // Safe to manipulate DOM
    document.querySelector('h1').textContent = 'Hello';
});

// load: slower, all resources loaded
window.addEventListener('load', () => {
    // Images loaded, can get dimensions
    const img = document.querySelector('img');
    console.log(img.naturalWidth);
});
```

## Quick Revision

- `DOMContentLoaded` - DOM ready (fast)
- `load` - all resources loaded (slow)
- `beforeunload` - user leaving
- `unload` - page unloaded
- Use DOMContentLoaded for DOM manipulation

---

## Related Topics

- [[What-is-Event]] - [[What-is-Event|Events]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
- [[Add-Listener]] - [[Add-Listener|Adding listeners]]
- [[What-is-DOM-Ready]] - [[What-is-DOM-Ready|DOM ready]]
