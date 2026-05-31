# Security

## Definition

Security in JavaScript involves **protecting applications** from attacks and vulnerabilities.

## Common Vulnerabilities

### XSS (Cross-Site Scripting)

```javascript
// ❌ Vulnerable
element.innerHTML = userInput;

// ✅ Safe
element.textContent = userInput;
```

### CSRF (Cross-Site Request Forgery)

```javascript
// Add CSRF token
fetch('/api/data', {
    method: 'POST',
    headers: {
        'X-CSRF-Token': token
    }
});
```

### SQL Injection

```javascript
// ❌ Vulnerable
const query = `SELECT * FROM users WHERE id = ${userId}`;

// ✅ Safe (parameterized)
const query = 'SELECT * FROM users WHERE id = ?';
db.query(query, [userId]);
```

## Best Practices

```javascript
// 1. Validate input
function validateEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

// 2. Sanitize output
function sanitize(input) {
    return input.replace(/<[^>]*>/g, '');
}

// 3. Use HTTPS
// Always use https:// in production

// 4. Store secrets securely
// Use environment variables, not hardcoded values
```

## Quick Revision

- XSS: sanitize user input
- CSRF: add tokens
- SQL injection: use parameterized queries
- Validate all input
- Use HTTPS
- Never hardcode secrets

---

## Related Topics

- [[What-is-XSS]] - [[What-is-XSS|XSS]]
- [[What-is-CSRF]] - [[What-is-CSRF|CSRF]]
- [[Security-Best-Practices]] - [[Security-Best-Practices|Security best practices]]
- [[Prevent-XSS]] - [[Prevent-XSS|Preventing XSS]]
- [[Store-Secrets]] - [[Store-Secrets|Storing secrets]]
