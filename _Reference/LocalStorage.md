# LocalStorage

## Definition
LocalStorage is a Web API that allows you to store key-value pairs in the browser with no expiration time. Data persists even after the browser is closed and reopened.

## Basic Operations

### Setting Items
```javascript
localStorage.setItem('username', 'JohnDoe');
localStorage.setItem('age', '30'); // Values must be strings
```

### Getting Items
```javascript
const username = localStorage.getItem('username');
console.log(username); // 'JohnDoe'
```

### Removing Items
```javascript
localStorage.removeItem('username');
```

### Clearing All
```javascript
localStorage.clear();
```

### Checking if Key Exists
```javascript
if (localStorage.getItem('username')) {
  console.log('Username exists');
}
```

## Working with Objects/Arrays
```javascript
// Storing objects
const user = { name: 'John', age: 30, hobbies: ['reading', 'gaming'] };
localStorage.setItem('user', JSON.stringify(user));

// Retrieving objects
const storedUser = JSON.parse(localStorage.getItem('user'));
console.log(storedUser.name); // 'John'
```

## Practical Examples

### Theme Preference
```javascript
// Save theme
function setTheme(theme) {
  localStorage.setItem('theme', theme);
  document.body.className = theme;
}

// Load theme on page load
const savedTheme = localStorage.getItem('theme');
if (savedTheme) {
  setTheme(savedTheme);
}
```

### Form Data Persistence
```javascript
// Auto-save form data
document.getElementById('email').addEventListener('input', (e) => {
  localStorage.setItem('formEmail', e.target.value);
});

// Restore form data on load
const savedEmail = localStorage.getItem('formEmail');
if (savedEmail) {
  document.getElementById('email').value = savedEmail;
}
```

## Common Use Cases
- User preferences (theme, language)
- Shopping cart contents
- Form data auto-save
- Caching API responses
- Storing authentication tokens (consider security)

## Common Mistakes
- Not converting objects to JSON before storing
- Assuming localStorage works in private/incognito mode (may have limits)
- Storing sensitive data (use secure alternatives)
- Exceeding storage limits (typically 5-10MB)
- Synchronous nature blocking the main thread

## Quick Revision Summary
- `setItem(key, value)` - stores data
- `getItem(key)` - retrieves data
- `removeItem(key)` - deletes a key
- `clear()` - removes all data
- All values are strings (use JSON.stringify/parse for objects)
- Persists across browser sessions

## Related Topics
- [[SessionStorage]]
- [[Cookies]]
- [[Web-APIs]]
- [[JSON]]
- [[Events]]
