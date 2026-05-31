# Validation

## Definition

Validation ensures **user input meets requirements**.

## Email Validation

```javascript
function isValidEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}
```

## Password Validation

```javascript
function isStrongPassword(password) {
    return /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$/.test(password);
}
```

## Quick Revision

- Validate all user input
- Use regex for patterns
- Show clear error messages
- Validate on client and server

---

## Related Topics

- [[What-is-Validation]] - [[What-is-Validation|Validation]]
- [[What-is-Validation]] - [[What-is-Validation|Validation]]
- [[Validation]] - [[Validation|Validation]]
- [[Sanitize-Input]] - [[Sanitize-Input|Input sanitization]]
