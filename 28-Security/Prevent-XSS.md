# Prevent XSS Attacks

## Definition

Preventing XSS (Cross-Site Scripting) involves **sanitizing input, encoding output, and using security headers** to block malicious script injection into your web applications.

## Prevention Techniques

| Technique | Purpose |
|-----------|---------|
| Input Sanitization | Remove malicious characters |
| Output Encoding | Convert special chars to HTML entities |
| Content Security Policy | Restrict script sources |
| HTTPOnly Cookies | Prevent JavaScript access |
| DOM Methods | Use safe alternatives to innerHTML |

## Code Examples

### 1. Use textContent Instead of innerHTML

```javascript
// ❌ Dangerous - allows script injection
element.innerHTML = userInput;

// ✅ Safe - treats input as plain text
element.textContent = userInput;

// ❌ Dangerous - using document.write
document.write(userInput);

// ✅ Safe - use DOM manipulation
const textNode = document.createTextNode(userInput);
element.appendChild(textNode);
```

### 2. Sanitize HTML with DOMPurify

```javascript
import DOMPurify from 'dompurify';

// ❌ Dangerous
element.innerHTML = userGeneratedHTML;

// ✅ Safe - sanitize before inserting
element.innerHTML = DOMPurify.sanitize(userGeneratedHTML);

// Configure DOMPurify
const clean = DOMPurify.sanitize(dirty, {
  ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'a'],
  ALLOWED_ATTR: ['href'],
  ALLOW_DATA_ATTR: false
});
```

### 3. Output Encoding

```javascript
function escapeHTML(str) {
  const div = document.createElement('div');
  div.appendChild(document.createTextNode(str));
  return div.innerHTML;
}

// Or use a dedicated library
import he from 'he';

const safe = he.encode('<script>alert("xss")</script>');
// Output: &lt;script&gt;alert(&quot;xss&quot;)&lt;/script&gt;
```

### 4. Content Security Policy Headers

```javascript
// Express.js CSP header
app.use((req, res, next) => {
  res.setHeader('Content-Security-Policy', `
    default-src 'self';
    script-src 'self' 'unsafe-inline';
    style-src 'self' 'unsafe-inline';
    img-src 'self' data: https:;
    font-src 'self';
    connect-src 'self';
    frame-ancestors 'none';
    base-uri 'self';
    form-action 'self'
  `);
  next();
});
```

### 5. Validate and Sanitize Input

```javascript
function sanitizeInput(input) {
  // Remove script tags
  let clean = input.replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '');

  // Remove event handlers
  clean = clean.replace(/\s*on\w+\s*=\s*["'][^"']*["']/gi, '');

  // Remove javascript: protocol
  clean = clean.replace(/javascript:/gi, '');

  return clean;
}

// Or use a validation library
import validator from 'validator';

const clean = validator.escape(userInput);
const isEmail = validator.isEmail(email);
const isURL = validator.isURL(url, { require_protocol: true });
```

### 6. Secure Cookie Settings

```javascript
// Set HTTPOnly cookies (cannot be accessed by JavaScript)
res.cookie('session', token, {
  httpOnly: true,    // Prevents XSS access
  secure: true,      // HTTPS only
  sameSite: 'strict' // Prevents CSRF
});
```

### 7. React-Specific Protection

```javascript
// React automatically escapes JSX
// ✅ Safe
function UserGreeting({ name }) {
  return <div>Hello, {name}</div>; // Auto-escaped
}

// ❌ Dangerous - dangerouslySetInnerHTML
function Comment({ html }) {
  return <div dangerouslySetInnerHTML={{ __html: html }} />;
}

// ✅ Safe alternative - render as text
function Comment({ text }) {
  return <div>{text}</div>;
}
```

## Common Use Cases

```javascript
// User comments - sanitize HTML
const cleanComment = DOMPurify.sanitize(comment);

// Search queries - encode output
const encodedQuery = he.encode(searchQuery);

// Profile bio - use contentEditable with sanitization
editor.addEventListener('input', (e) => {
  const clean = DOMPurify.sanitize(e.target.innerHTML);
  e.target.innerHTML = clean;
});

// URL parameters - validate before use
const params = new URLSearchParams(window.location.search);
const userId = parseInt(params.get('id'), 10);
if (isNaN(userId)) throw new Error('Invalid user ID');
```

## Common Mistakes

| Mistake | Risk |
|---------|------|
| Using innerHTML with user data | Script injection |
| Not sanitizing rich text | Stored XSS |
| Trusting URL parameters | Reflected XSS |
| Missing CSP headers | No defense layer |
| Using eval() | Remote code execution |
| Allowing javascript: URLs | Script execution |
| Not encoding output | DOM manipulation |

## Quick Revision

- Always sanitize user input before rendering
- Use `textContent` instead of `innerHTML`
- Implement Content Security Policy headers
- Use DOMPurify for rich text sanitization
- Set HTTPOnly and Secure flags on cookies
- Never use `eval()` or `document.write()`
- Validate and encode all output
- Use `he` library for HTML entity encoding
- React/JSX auto-escapes but avoid `dangerouslySetInnerHTML`
- Multiple layers of defense are essential

---

## Related Topics

- [[What-is-XSS]] - XSS overview and types
- [[Sanitize-Input]] - Input sanitization techniques
- [[What-is-CSP]] - Content Security Policy
- [[What-is-CSRF]] - Cross-Site Request Forgery
- [[What-is-Authentication]] - Secure authentication
- [[What-is-HTTPS]] - Secure HTTP connections
