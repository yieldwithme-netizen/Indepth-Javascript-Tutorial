# LocalStorage

LocalStorage is a Web Storage API that allows you to store key-value pairs in the browser with no expiration time.

## Basic Operations

```javascript
// Set item
localStorage.setItem('username', 'JohnDoe');

// Get item
const username = localStorage.getItem('username');
console.log(username); // 'JohnDoe'

// Remove item
localStorage.removeItem('username');

// Clear all
localStorage.clear();

// Get length
const count = localStorage.length;
console.log(count); // 0
```

## Working with Objects

```javascript
// Store object
const user = {
  name: 'John',
  age: 30,
  preferences: { theme: 'dark' }
};
localStorage.setItem('user', JSON.stringify(user));

// Retrieve object
const storedUser = JSON.parse(localStorage.getItem('user'));
console.log(storedUser.name); // 'John'
```

## Practical Examples

### User Preferences

```javascript
function savePreferences(prefs) {
  localStorage.setItem('preferences', JSON.stringify(prefs));
}

function loadPreferences(defaults) {
  const stored = localStorage.getItem('preferences');
  return stored ? { ...defaults, ...JSON.parse(stored) } : defaults;
}

const defaultPrefs = { theme: 'light', language: 'en' };
const userPrefs = loadPreferences(defaultPrefs);
```

### Shopping Cart

```javascript
function getCart() {
  const cart = localStorage.getItem('cart');
  return cart ? JSON.parse(cart) : [];
}

function addToCart(item) {
  const cart = getCart();
  cart.push(item);
  localStorage.setItem('cart', JSON.stringify(cart));
}

function clearCart() {
  localStorage.removeItem('cart');
}
```

## Storage Limits

```javascript
// Typical limit: 5-10MB per domain
// Check available space
function getStorageSize() {
  let total = 0;
  for (let i = 0; i < localStorage.length; i++) {
    const key = localStorage.key(i);
    total += localStorage.getItem(key).length * 2; // UTF-16
  }
  return total;
}
```

## Common Use Cases

- User preferences
- Shopping carts
- Form data persistence
- Caching data
- Authentication tokens (with caution)

## Common Mistakes

- Storing sensitive data (passwords, tokens)
- Not handling JSON parse errors
- Exceeding storage limits without error handling
- Blocking the main thread with large data
- Not checking for localStorage availability

## Related Topics

- [[SessionStorage]]
- [[Web Storage API]]
- [[JSON]]
- [[Cookies]]
- [[IndexedDB]]

## Quick Revision

- LocalStorage persists data with no expiration
- Use `setItem`, `getItem`, `removeItem`, `clear`
- Store objects using `JSON.stringify` and `JSON.parse`
- Typical limit is 5-10MB per domain
- Not suitable for sensitive data
- Check availability before using
