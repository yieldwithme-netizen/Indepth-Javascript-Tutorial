# Data Validation

## Definition

Data validation **ensures data meets requirements**.

## Example

```javascript
function validate(data) {
    const errors = [];
    if (!data.name) errors.push('Name required');
    if (!data.email.includes('@')) errors.push('Invalid email');
    return { isValid: errors.length === 0, errors };
}
```

## Quick Revision

- Validate before processing
- Check required fields
- Validate formats
- Return clear errors

---

## Related Topics

- [[What-is-Validation]] - [[What-is-Validation|Validation]]
- [[Validation]] - [[Validation|Validation]]
- [[Data-Validation]] - [[Data-Validation|Data validation]]
- [[Input-Validation]] - [[Input-Validation|Input validation]]
