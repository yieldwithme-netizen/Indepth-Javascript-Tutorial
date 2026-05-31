# LocalStorage

## Definition

**LocalStorage** is a Web Storage API that allows you to store key-value pairs in the browser with no expiration date. Data persists even after the browser is closed and reopened. It provides a simple way to save data on the client side without cookies or server requests.

LocalStorage is limited to storing strings (usually JSON-serialized data) and has a storage limit of approximately 5-10MB depending on the browser.

---

## Syntax

```javascript
// Set item
localStorage.setItem('key', 'value');

// Get item
const value = localStorage.getItem('key');

// Remove item
localStorage.removeItem('key');

// Clear all items
localStorage.clear();

// Get number of items
const count = localStorage.length;

// Get key by index
const key = localStorage.key(0);
```

---

## Code Examples

### Basic Operations
```javascript
// Store data
localStorage.setItem('username', 'Alice');

// Retrieve data
const username = localStorage.getItem('username');
console.log(username); // Output: Alice

// Remove data
localStorage.removeItem('username');

// Clear all data
localStorage.clear();
```

### Storing Objects and Arrays
```javascript
// Objects must be serialized
const user = { name: 'Alice', age: 30, city: 'New York' };
localStorage.setItem('user', JSON.stringify(user));

// Retrieve and parse
const storedUser = JSON.parse(localStorage.getItem('user'));
console.log(storedUser.name); // Output: Alice

// Arrays
const colors = ['red', 'green', 'blue'];
localStorage.setItem('colors', JSON.stringify(colors));

const storedColors = JSON.parse(localStorage.getItem('colors'));
console.log(storedColors); // Output: ['red', 'green', 'blue']
```

### Safe JSON Parsing
```javascript
function safeGetJSON(key, defaultValue = null) {
  try {
    const item = localStorage.getItem(key);
    return item ? JSON.parse(item) : defaultValue;
  } catch (error) {
    console.error(`Error parsing ${key}:`, error);
    return defaultValue;
  }
}

// Usage
const settings = safeGetJSON('settings', { theme: 'dark' });
console.log(settings.theme); // Output: dark
```

### Checking if Key Exists
```javascript
// Method 1: Check for null
if (localStorage.getItem('token') !== null) {
  console.log('Token exists');
}

// Method 2: Use in operator (works with localStorage)
if ('token' in localStorage) {
  console.log('Token exists');
}

// Method 3: Check length
if (localStorage.length > 0) {
  console.log('Storage has items');
}
```

### Session Persistence Example
```javascript
// Save form data
function saveFormData() {
  const formData = {
    name: document.getElementById('name').value,
    email: document.getElementById('email').value,
    timestamp: Date.now()
  };
  localStorage.setItem('formDraft', JSON.stringify(formData));
}

// Load form data
function loadFormData() {
  const draft = JSON.parse(localStorage.getItem('formDraft'));
  if (draft) {
    document.getElementById('name').value = draft.name;
    document.getElementById('email').value = draft.email;
  }
}

// Auto-save on input
document.querySelectorAll('input').forEach(input => {
  input.addEventListener('input', saveFormData);
});
```

### Shopping Cart Example
```javascript
class ShoppingCart {
  constructor() {
    this.items = JSON.parse(localStorage.getItem('cart')) || [];
  }

  addItem(item) {
    this.items.push(item);
    this.save();
  }

  removeItem(index) {
    this.items.splice(index, 1);
    this.save();
  }

  getTotal() {
    return this.items.reduce((sum, item) => sum + item.price, 0);
  }

  save() {
    localStorage.setItem('cart', JSON.stringify(this.items));
  }

  clear() {
    this.items = [];
    localStorage.removeItem('cart');
  }
}

// Usage
const cart = new ShoppingCart();
cart.addItem({ name: 'Laptop', price: 999 });
cart.addItem({ name: 'Mouse', price: 25 });
console.log(cart.getTotal()); // Output: 1024
```

### User Preferences
```javascript
class UserPreferences {
  constructor() {
    this.defaults = {
      theme: 'light',
      language: 'en',
      notifications: true
    };
    this.prefs = this.load();
  }

  load() {
    const stored = localStorage.getItem('preferences');
    return stored ? { ...this.defaults, ...JSON.parse(stored) } : { ...this.defaults };
  }

  save() {
    localStorage.setItem('preferences', JSON.stringify(this.prefs));
  }

  get(key) {
    return this.prefs[key];
  }

  set(key, value) {
    this.prefs[key] = value;
    this.save();
  }
}

// Usage
const prefs = new UserPreferences();
prefs.set('theme', 'dark');
console.log(prefs.get('theme')); // Output: dark
```

### Storage Event Listener
```javascript
// Listen for changes in other tabs/windows
window.addEventListener('storage', (event) => {
  console.log('Storage changed:');
  console.log('Key:', event.key);
  console.log('Old value:', event.oldValue);
  console.log('New value:', event.newValue);
});
```

### Comparing with SessionStorage
```javascript
// LocalStorage - persists across sessions
localStorage.setItem('permanent', 'data');

// SessionStorage - cleared when tab closes
sessionStorage.setItem('temporary', 'data');

// Both have same API
console.log(localStorage.length);
console.log(sessionStorage.length);
```

---

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| **User Preferences** | Theme, language, settings |
| **Form Drafts** | Auto-save form data |
| **Shopping Cart** | Persist cart across page reloads |
| **Authentication Tokens** | Store JWT or session tokens |
| **Caching** | Cache API responses locally |
| **Game State** | Save progress in browser games |

---

## Common Mistakes

### 1. Not Stringifying Objects
```javascript
// WRONG - Stores "[object Object]"
const user = { name: 'Alice' };
localStorage.setItem('user', user);

// CORRECT
localStorage.setItem('user', JSON.stringify(user));
const stored = JSON.parse(localStorage.getItem('user'));
```

### 2. Not Handling Missing Keys
```javascript
// WRONG - May cause errors
const user = JSON.parse(localStorage.getItem('user'));
console.log(user.name); // Error if user is null

// CORRECT - Safe access
const user = JSON.parse(localStorage.getItem('user') || '{}');
console.log(user?.name);
```

### 3. Storing Sensitive Data
```javascript
// BAD - Never store passwords or sensitive data
localStorage.setItem('password', 'secret123');

// GOOD - Store tokens from secure httpOnly cookies
// Let the server handle sensitive data
```

---

## Quick Revision Summary

- LocalStorage stores key-value pairs as strings
- Data persists across browser sessions (no expiration)
- Maximum storage: ~5-10MB depending on browser
- Always use `JSON.stringify()` to store objects/arrays
- Always use `JSON.parse()` with try-catch for safe retrieval
- Never store sensitive data (passwords, secrets) in localStorage
- Use `sessionStorage` for temporary data that should clear when tab closes
- Listen for `storage` event for cross-tab synchronization

---

## Related Topics

- [[JavaScript]] - JavaScript language overview
- [[function]] - Functions for storage operations
- [[Function-Scope-and-Closures]] - Scope in storage utilities
- [[let]] - Variable declarations for storage keys
- [[Logical-Operators]] - Conditional checks for stored data
