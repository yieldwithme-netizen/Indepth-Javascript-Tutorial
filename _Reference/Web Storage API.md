# Web Storage API

## Definition

Web Storage API stores **client-side data** in the browser.

## LocalStorage

```javascript
// Store
localStorage.setItem('name', 'John');

// Get
const name = localStorage.getItem('name');

// Remove
localStorage.removeItem('name');

// Clear
localStorage.clear();
```

## SessionStorage

```javascript
// Same API as localStorage
sessionStorage.setItem('name', 'John');
const name = sessionStorage.getItem('name');
```

## Quick Revision

- LocalStorage: persists until cleared
- SessionStorage: session only
- Both: ~5MB limit
- Store objects with JSON.stringify/parse

---

## Related Topics

- [[What-is-LocalStorage]] - [[What-is-LocalStorage|LocalStorage]]
- [[What-is-SessionStorage]] - [[What-is-SessionStorage|SessionStorage]]
- [[Local-Storage]] - [[Local-Storage|Local storage]]
- [[Session-Storage]] - [[Session-Storage|Session storage]]
- [[Web Storage API]] - [[Web Storage API|Web storage]]
