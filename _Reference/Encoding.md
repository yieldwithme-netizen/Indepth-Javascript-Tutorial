# Encoding in JavaScript

## Definition

Encoding is the process of converting data from one format to another. In JavaScript, encoding is commonly used for URL encoding, Base64 encoding, HTML entity encoding, and character encoding to ensure data is safely transmitted and displayed across different systems.

## URL Encoding

### 1. encodeURIComponent

```javascript
// Encodes special characters for URL parameters
const url = 'https://example.com/search';
const query = 'Hello World & Goodbye!';
const encoded = encodeURIComponent(query);

console.log(encoded); // "Hello%20World%20%26%20Goodbye!"
console.log(`${url}?q=${encoded}`);
// "https://example.com/search?q=Hello%20World%20%26%20Goodbye!"
```

### 2. encodeURI

```javascript
// Encodes full URI but keeps valid URI characters
const url = 'https://example.com/path?q=hello world';
const encoded = encodeURI(url);

console.log(encoded);
// "https://example.com/path?q=hello%20world"
// Note:保留: / ? # etc.
```

### 3. Decoding

```javascript
const encoded = 'Hello%20World%20%26%20Goodbye%21';

// Decode component
const decoded1 = decodeURIComponent(encoded);
console.log(decoded1); // "Hello World & Goodbye!"

// Decode full URI
const decoded2 = decodeURI(encoded);
console.log(decoded2); // "Hello%20World%20%26%20Goodbye%21"
```

### 4. Common Use Cases

```javascript
// Building search URL
function buildSearchURL(baseUrl, query) {
  return `${baseUrl}?q=${encodeURIComponent(query)}`;
}

// Parsing URL parameters
function getURLParam(param) {
  const params = new URLSearchParams(window.location.search);
  return params.get(param);
}

// Encoding form data
const formData = {
  name: 'John Doe',
  email: 'john@example.com',
  message: 'Hello & Goodbye!'
};

const encoded = Object.entries(formData)
  .map(([key, value]) => `${key}=${encodeURIComponent(value)}`)
  .join('&');

console.log(encoded);
// "name=John%20Doe&email=john%40example.com&message=Hello%20%26%20Goodbye!"
```

## Base64 Encoding

### 1. btoa (Binary to ASCII)

```javascript
// Encode string to Base64
const original = 'Hello, World!';
const encoded = btoa(original);

console.log(encoded); // "SGVsbG8sIFdvcmxkIQ=="
```

### 2. atob (ASCII to Binary)

```javascript
// Decode Base64 to string
const encoded = 'SGVsbG8sIFdvcmxkIQ==';
const decoded = atob(encoded);

console.log(decoded); // "Hello, World!"
```

### 3. Encoding Unicode

```javascript
// Handle Unicode characters
function encodeUnicode(str) {
  return btoa(encodeURIComponent(str).replace(/%([0-9A-F]{2})/g,
    (match, p1) => String.fromCharCode('0x' + p1)
  ));
}

function decodeUnicode(str) {
  return decodeURIComponent(atob(str).split('').map(c =>
    '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2)
  ).join(''));
}

const unicode = '你好世界';
const encoded = encodeUnicode(unicode);
console.log(encoded); // Base64 encoded

const decoded = decodeUnicode(encoded);
console.log(decoded); // "你好世界"
```

### 4. Encoding Objects

```javascript
// Encode object to Base64 JSON
function encodeObject(obj) {
  return btoa(JSON.stringify(obj));
}

// Decode Base64 to object
function decodeObject(encoded) {
  return JSON.parse(atob(encoded));
}

const user = { name: 'Alice', age: 30 };
const encoded = encodeObject(user);
console.log(encoded); // Base64 string

const decoded = decodeObject(encoded);
console.log(decoded); // { name: 'Alice', age: 30 }
```

## HTML Entity Encoding

### 1. Encoding HTML

```javascript
function encodeHTML(str) {
  const htmlEntities = {
    '&': '&amp;',
    '<': '&lt;',
    '>': '&gt;',
    '"': '&quot;',
    "'": '&#39;'
  };

  return str.replace(/[&<>"']/g, char => htmlEntities[char]);
}

const unsafe = '<script>alert("XSS")</script>';
const safe = encodeHTML(unsafe);
console.log(safe);
// "&lt;script&gt;alert(&quot;XSS&quot;)&lt;/script&gt;"
```

### 2. Decoding HTML

```javascript
function decodeHTML(str) {
  const htmlEntities = {
    '&amp;': '&',
    '&lt;': '<',
    '&gt;': '>',
    '&quot;': '"',
    '&#39;': "'"
  };

  return str.replace(/&amp;|&lt;|&gt;|&quot;|&#39;/g,
    entity => htmlEntities[entity]
  );
}

const encoded = '&lt;div&gt;Hello&lt;/div&gt;';
const decoded = decodeHTML(encoded);
console.log(decoded); // "<div>Hello</div>"
```

### 3. Preventing XSS

```javascript
// User input
const userInput = '<img src=x onerror=alert(1)>';

// Safe rendering
function safeRender(container, content) {
  container.textContent = content; // Auto-escapes HTML
}

// Or use DOMPurify
import DOMPurify from 'dompurify';
const clean = DOMPurify.sanitize(userInput);
```

## Character Encoding

### 1. TextEncoder

```javascript
// Encode string to Uint8Array (UTF-8)
const encoder = new TextEncoder();
const str = 'Hello, World! 你好';

const encoded = encoder.encode(str);
console.log(encoded); // Uint8Array [72, 101, 108, ...]
```

### 2. TextDecoder

```javascript
// Decode Uint8Array to string
const decoder = new TextDecoder('utf-8');
const uint8Array = new Uint8Array([72, 101, 108, 108, 111]);

const decoded = decoder.decode(uint8Array);
console.log(decoded); // "Hello"
```

### 3. Hex Encoding

```javascript
// String to Hex
function stringToHex(str) {
  return Array.from(str)
    .map(c => c.charCodeAt(0).toString(16).padStart(2, '0'))
    .join('');
}

// Hex to String
function hexToString(hex) {
  return hex.match(/.{1,2}/g)
    .map(byte => String.fromCharCode(parseInt(byte, 16)))
    .join('');
}

console.log(stringToHex('Hello')); // "48656c6c6f"
console.log(hexToString('48656c6c6f')); // "Hello"
```

## Common Use Cases

### Secure Data Transmission

```javascript
// JWT-like token encoding
function createToken(payload) {
  const header = btoa(JSON.stringify({ alg: 'HS256', typ: 'JWT' }));
  const body = btoa(JSON.stringify(payload));
  const signature = btoa('signature'); // In real use, use HMAC

  return `${header}.${body}.${signature}`;
}

function decodeToken(token) {
  const [header, body, signature] = token.split('.');
  return {
    header: JSON.parse(atob(header)),
    payload: JSON.parse(atob(body))
  };
}
```

### Data Storage

```javascript
// Store complex data in localStorage
function saveToStorage(key, data) {
  const encoded = btoa(JSON.stringify(data));
  localStorage.setItem(key, encoded);
}

function loadFromStorage(key) {
  const encoded = localStorage.getItem(key);
  return encoded ? JSON.parse(atob(encoded)) : null;
}

// Usage
saveToStorage('user', { name: 'Alice', preferences: { theme: 'dark' } });
const user = loadFromStorage('user');
```

### API Authentication

```javascript
// Basic Auth header
function createBasicAuth(username, password) {
  const credentials = `${username}:${password}`;
  const encoded = btoa(credentials);
  return `Basic ${encoded}`;
}

const authHeader = createBasicAuth('user', 'pass123');
// "Basic dXNlcjpwYXNzMTIz"

// Add to fetch
fetch(url, {
  headers: {
    'Authorization': authHeader
  }
});
```

## Common Mistakes

### 1. Using btoa with Unicode

```javascript
// WRONG: btoa fails with Unicode
const str = '你好';
btoa(str); // InvalidCharacterError

// RIGHT: Use proper encoding
function safeBtoa(str) {
  return btoa(encodeURIComponent(str).replace(/%([0-9A-F]{2})/g,
    (match, p1) => String.fromCharCode('0x' + p1)
  ));
}
```

### 2. Not Encoding URL Parameters

```javascript
// WRONG: Special characters break URL
const search = 'hello world & more';
window.location.href = `/search?q=${search}`;
// URL becomes: /search?q=hello world & more (broken)

// RIGHT: Encode properly
window.location.href = `/search?q=${encodeURIComponent(search)}`;
```

### 3. Double Encoding

```javascript
// WRONG: Encoding already encoded string
const once = encodeURIComponent('hello world');
const twice = encodeURIComponent(once);
console.log(twice); // "hello%2520world" (wrong)

// RIGHT: Encode only once
const value = 'hello world';
const encoded = encodeURIComponent(value); // "hello%20world"
```

## Quick Revision Summary

- **URL Encoding**: `encodeURIComponent` for parameters, `encodeURI` for full URLs
- **Base64**: `btoa` to encode, `atob` to decode
- **HTML Encoding**: Escape `<`, `>`, `&`, `"`, `'` to prevent XSS
- **TextEncoder/Decoder**: UTF-8 encoding for binary data
- **Always encode** user input before displaying in HTML
- **Always encode** special characters in URLs

## Related Topics

- [[URL-Handling]] - Working with URLs
- [[Fetch-API]] - Making HTTP requests
- [[Security-Best-Practices]] - XSS and injection prevention
- [[LocalStorage]] - Client-side storage
- [[JSON]] - JSON parsing and stringifying
- [[Binary-Data]] - Working with binary data
