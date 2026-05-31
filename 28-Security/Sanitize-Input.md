# Sanitize User Input

## Definition

Input sanitization is **cleaning and validating user input** to prevent security vulnerabilities like XSS, SQL injection, and command injection before processing or storing data.

## Sanitization Methods

| Method | Purpose |
|--------|---------|
| Whitelisting | Allow only safe characters |
| Blacklisting | Block known dangerous patterns |
| Encoding | Convert special characters |
| Validation | Check input format/type |
| Truncation | Limit input length |

## Code Examples

### 1. Basic String Sanitization

```javascript
// Remove all HTML tags
function stripHTML(input) {
  return input.replace(/<[^>]*>/g, '');
}

// Remove script tags specifically
function removeScripts(input) {
  return input.replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '');
}

// Remove event handlers
function removeEventHandlers(input) {
  return input.replace(/\s*on\w+\s*=\s*(["'])(.*?)\1/gi, '');
}

// Remove javascript: protocol
function removeJSProtocol(input) {
  return input.replace(/javascript:/gi, '');
}
```

### 2. Email Validation

```javascript
function validateEmail(email) {
  const regex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
  return regex.test(email);
}

// Using validator.js library
import validator from 'validator';

// Check if email is valid
const isValid = validator.isEmail('user@example.com');

// Normalize email
const normalized = validator.normalizeEmail('USER@EXAMPLE.COM');
// Output: user@example.com
```

### 3. URL Validation

```javascript
function validateURL(url) {
  try {
    const parsed = new URL(url);
    // Only allow http and https
    return ['http:', 'https:'].includes(parsed.protocol);
  } catch {
    return false;
  }
}

// Using validator.js
import validator from 'validator';

const isValid = validator.isURL('https://example.com', {
  protocols: ['http', 'https'],
  require_protocol: true,
  require_valid_protocol: true
});
```

### 4. Number Validation

```javascript
function sanitizeNumber(input, min = 0, max = Infinity) {
  const num = parseInt(input, 10);
  if (isNaN(num)) return null;
  return Math.min(Math.max(num, min), max);
}

// Usage
const age = sanitizeNumber(userInput, 0, 150);
const quantity = sanitizeNumber(orderQty, 1, 100);
```

### 5. DOMPurify for Rich Text

```javascript
import DOMPurify from 'dompurify';

// Basic sanitization
const clean = DOMPurify.sanitize('<img src=x onerror=alert(1)>');
// Output: <img src="x">

// Allow specific tags
const allowed = DOMPurify.sanitize(userHTML, {
  ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'p', 'br'],
  ALLOWED_ATTR: []
});

// With custom config
const strict = DOMPurify.sanitize(dirty, {
  ALLOWED_TAGS: ['p', 'br', 'ul', 'ol', 'li'],
  ALLOWED_ATTR: ['class'],
  FORBID_TAGS: ['style', 'script'],
  FORBID_ATTR: ['onclick', 'onerror']
});
```

### 6. SQL Injection Prevention

```javascript
// ❌ Dangerous - string concatenation
const query = `SELECT * FROM users WHERE id = ${userId}`;

// ✅ Safe - parameterized queries (using mysql2)
const [rows] = await connection.execute(
  'SELECT * FROM users WHERE id = ?',
  [userId]
);

// ✅ Safe - using ORM like Prisma
const user = await prisma.user.findUnique({
  where: { id: parseInt(userId) }
});
```

### 7. Command Injection Prevention

```javascript
const { exec } = require('child_process');

// ❌ Dangerous - direct user input in command
exec(`ping ${userInput}`);

// ✅ Safe - validate and use array form
const { execFile } = require('child_process');

const allowedHosts = ['google.com', 'github.com'];
if (allowedHosts.includes(userInput)) {
  execFile('ping', ['-c', '4', userInput]);
} else {
  throw new Error('Invalid host');
}
```

### 8. File Upload Sanitization

```javascript
function sanitizeFilename(filename) {
  // Remove path traversal
  let clean = filename.replace(/\.\.\//g, '');

  // Remove special characters
  clean = clean.replace(/[^a-zA-Z0-9._-]/g, '_');

  // Limit length
  const ext = clean.split('.').pop();
  const name = clean.slice(0, 100 - ext.length - 1);
  return `${name}.${ext}`;
}

// Validate file type
const allowedTypes = ['image/jpeg', 'image/png', 'image/gif'];
const file = req.files.upload;

if (!allowedTypes.includes(file.mimetype)) {
  return res.status(400).json({ error: 'Invalid file type' });
}
```

## Common Use Cases

```javascript
// Form input sanitization
function sanitizeFormInput(data) {
  return {
    name: validator.escape(data.name || ''),
    email: validator.normalizeEmail(data.email || ''),
    age: parseInt(data.age, 10) || 0,
    bio: DOMPurify.sanitize(data.bio || ''),
    url: validator.isURL(data.url) ? data.url : ''
  };
}

// API query parameter sanitization
function sanitizeQueryParams(query) {
  return {
    page: Math.max(1, parseInt(query.page, 10) || 1),
    limit: Math.min(100, parseInt(query.limit, 10) || 10),
    sort: ['name', 'date', 'price'].includes(query.sort) ? query.sort : 'name'
  };
}
```

## Common Mistakes

| Mistake | Risk |
|---------|------|
| Only sanitizing on client side | Server still vulnerable |
| Using blacklists only | Easy to bypass |
| Not handling Unicode | Bypass with special chars |
| Trusting client validation | Easily bypassed |
| Forgetting file uploads | Malicious file execution |
| Not encoding output | XSS in display |
| Ignoring null bytes | Injection attacks |

## Quick Revision

- Always validate AND sanitize input
- Use whitelisting over blacklisting
- Validate on both client and server
- Use DOMPurify for HTML content
- Parameterized queries prevent SQL injection
- Validate file types and sanitize filenames
- Encode output when displaying user data
- Use libraries like `validator.js` for complex validation
- Never trust user input - always verify

---

## Related Topics

- [[Prevent-XSS]] - XSS prevention techniques
- [[What-is-XSS]] - XSS overview
- [[What-is-CSP]] - Content Security Policy
- [[What-is-CSRF]] - Cross-Site Request Forgery
- [[Store-Secrets]] - Secure secret storage
- [[What-is-OWASP]] - OWASP security guidelines
