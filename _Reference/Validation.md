# Validation

## Definition

Validation ensures **user input meets requirements** before processing.

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

## Form Validation

```javascript
function validateForm(data) {
    const errors = {};
    
    if (!data.name) errors.name = "Name is required";
    if (!isValidEmail(data.email)) errors.email = "Invalid email";
    if (!isStrongPassword(data.password)) errors.password = "Weak password";
    
    return {
        isValid: Object.keys(errors).length === 0,
        errors
    };
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
- [[Validation]] - [[Validation|Validation]]
- [[Handle-Form]] - [[Handle-Form|Form handling]]
- [[Sanitize-Input]] - [[Sanitize-Input|Input sanitization]]
