# Input Validation

## Definition

Input validation **checks user input** before processing.

## Example

```javascript
function validateEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

function validatePassword(password) {
    return /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$/.test(password);
}
```

## Quick Revision

- Validate all input
- Use regex for patterns
- Show clear errors
- Validate client and server

---

## Related Topics

- [[What-is-Validation]] - [[What-is-Validation|Validation]]
- [[Validation]] - [[Validation|Validation]]
- [[Input-Validation]] - [[Input-Validation|Input validation]]
- [[Sanitize-Input]] - [[Sanitize-Input|Input sanitization]]
