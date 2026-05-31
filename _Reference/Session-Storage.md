# Session Storage

## Definition

SessionStorage stores data that **persists for one browser session** (until tab/window closes).

## Basic Usage

```javascript
// Store data
sessionStorage.setItem('name', 'John');

// Get data
const name = sessionStorage.getItem('name'); // "John"

// Remove data
sessionStorage.removeItem('name');

// Clear all
sessionStorage.clear();

// Get length
sessionStorage.length;
```

## Working with Objects

```javascript
// Store object
const user = { name: "John", age: 30 };
sessionStorage.setItem('user', JSON.stringify(user));

// Get object
const stored = JSON.parse(sessionStorage.getItem('user'));
```

## SessionStorage vs LocalStorage

| Feature | SessionStorage | LocalStorage |
|---------|----------------|--------------|
| Duration | Session only | Until cleared |
| Scope | Tab only | All tabs |
| Size | ~5MB | ~5MB |

## Quick Revision

- SessionStorage = session-only storage
- `setItem()`, `getItem()`, `removeItem()`, `clear()`
- Scope: same tab, same origin
- Data lost when tab closes
- Store objects with JSON.stringify/parse

---

## Related Topics

- [[What-is-SessionStorage]] - [[What-is-SessionStorage|SessionStorage]] overview
- [[What-is-LocalStorage]] - [[What-is-LocalStorage|LocalStorage]]
- [[Local-Storage]] - [[Local-Storage|Local storage]]
- [[What-is-Cookies]] - [[What-is-Cookies|Cookies]]
