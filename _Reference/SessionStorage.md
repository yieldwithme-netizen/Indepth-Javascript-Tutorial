# SessionStorage

## Definition

SessionStorage stores data that **persists for one browser session**.

## Basic Usage

```javascript
// Store
sessionStorage.setItem('name', 'John');

// Get
const name = sessionStorage.getItem('name');

// Remove
sessionStorage.removeItem('name');

// Clear
sessionStorage.clear();

// Length
sessionStorage.length;
```

## Quick Revision

- SessionStorage = session-only storage
- `setItem()`, `getItem()`, `removeItem()`, `clear()`
- Scope: same tab, same origin
- Data lost when tab closes

---

## Related Topics

- [[What-is-SessionStorage]] - [[What-is-SessionStorage|SessionStorage]]
- [[SessionStorage]] - [[SessionStorage|SessionStorage]]
- [[Session-Storage]] - [[Session-Storage|Session storage]]
- [[What-is-LocalStorage]] - [[What-is-LocalStorage|LocalStorage]]
