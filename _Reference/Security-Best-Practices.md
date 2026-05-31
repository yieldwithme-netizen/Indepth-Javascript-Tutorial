# Security Best Practices

## Input Validation

```javascript
// Sanitize user input
function sanitize(input) {
    return input
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#x27;");
}

// Validate email
function isValidEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}
```

## XSS Prevention

```javascript
// ❌ Vulnerable
element.innerHTML = userInput;

// ✅ Safe
element.textContent = userInput;

// Or use DOMPurify
import DOMPurify from 'dompurize';
element.innerHTML = DOMPurify.sanitize(userInput);
```

## CSRF Prevention

```javascript
// Add CSRF token to forms
const token = document.querySelector('meta[name="csrf-token"]').content;

fetch('/api/data', {
    method: 'POST',
    headers: {
        'X-CSRF-Token': token
    }
});
```

## Secure Storage

```javascript
// ❌ Don't store secrets in localStorage
localStorage.setItem('token', secretToken);

// ✅ Use httpOnly cookies (server-side)
// Set-Cookie: token=xxx; HttpOnly; Secure; SameSite=Strict
```

## Quick Revision

- Validate and sanitize all inputs
- Use textContent instead of innerHTML
- Add CSRF tokens to forms
- Never store secrets in localStorage
- Use httpOnly cookies for tokens

---

## Related Topics

- [[What-is-XSS]] - [[What-is-XSS|XSS]]
- [[Prevent-XSS]] - [[Prevent-XSS|Preventing XSS]]
- [[What-is-CSRF]] - [[What-is-CSRF|CSRF]]
- [[Sanitize-Input]] - [[Sanitize-Input|Input sanitization]]
- [[Store-Secrets]] - [[Store-Secrets|Storing secrets]]
