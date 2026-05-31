# SessionStorage

SessionStorage is a Web Storage API that stores data for the duration of a browser session. Data is cleared when the tab or window is closed.

## Basic Operations

```javascript
// Set item
sessionStorage.setItem('token', 'abc123');

// Get item
const token = sessionStorage.getItem('token');
console.log(token); // 'abc123'

// Remove item
sessionStorage.removeItem('token');

// Clear all
sessionStorage.clear();

// Get length
const count = sessionStorage.length;
console.log(count); // 0
```

## Working with Objects

```javascript
// Store object
const formData = {
  step: 1,
  data: { name: 'John', email: 'john@example.com' }
};
sessionStorage.setItem('formProgress', JSON.stringify(formData));

// Retrieve object
const stored = JSON.parse(sessionStorage.getItem('formProgress'));
console.log(stored.step); // 1
```

## Practical Examples

### Multi-Step Form

```javascript
function saveFormStep(step, data) {
  const progress = { step, data, timestamp: Date.now() };
  sessionStorage.setItem('formProgress', JSON.stringify(progress));
}

function loadFormStep() {
  const stored = sessionStorage.getItem('formProgress');
  if (stored) {
    return JSON.parse(stored);
  }
  return { step: 1, data: {} };
}

// Usage
saveFormStep(2, { name: 'John', email: 'john@example.com' });
const progress = loadFormStep();
console.log(progress.step); // 2
```

### Temporary State

```javascript
function setTemporaryState(key, value, ttl = 300000) {
  const item = {
    value,
    expiry: Date.now() + ttl
  };
  sessionStorage.setItem(key, JSON.stringify(item));
}

function getTemporaryState(key) {
  const itemStr = sessionStorage.getItem(key);
  if (!itemStr) return null;

  const item = JSON.parse(itemStr);
  if (Date.now() > item.expiry) {
    sessionStorage.removeItem(key);
    return null;
  }
  return item.value;
}
```

## SessionStorage vs LocalStorage

```javascript
// SessionStorage - per tab/window, cleared on close
sessionStorage.setItem('tabData', 'value');

// LocalStorage - persists across sessions
localStorage.setItem('userData', 'value');

// Same-origin rule applies to both
```

## Common Use Cases

- Multi-step form progress
- Temporary state management
- Shopping cart during session
- UI state (scroll position, open tabs)
- One-time notifications

## Common Mistakes

- Assuming data persists after closing tab
- Not handling JSON parse errors
- Storing too much data
- Not checking availability
- Using for sensitive data

## Related Topics

- [[LocalStorage]]
- [[Web Storage API]]
- [[JSON]]
- [[Cookies]]
- [[State Management]]

## Quick Revision

- SessionStorage data persists for one session only
- Data clears when tab/window closes
- Same API as LocalStorage: `setItem`, `getItem`, `removeItem`, `clear`
- Per-origin and per-tab storage
- Useful for temporary form data and UI state
- Not suitable for sensitive information
