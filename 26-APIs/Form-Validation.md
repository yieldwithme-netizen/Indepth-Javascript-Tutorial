# Form Validation

## Definition

Form validation **checks user input** before submission.

## Example

```javascript
form.addEventListener('submit', (e) => {
    e.preventDefault();
    const email = form.email.value;
    
    if (!email.includes('@')) {
        alert('Invalid email');
        return;
    }
    
    // Submit form
});
```

## Quick Revision

- Validate on submit
- Check required fields
- Validate formats
- Show error messages

---

## Related Topics

- [[What-is-Validation]] - [[What-is-Validation|Validation]]
- [[Validation]] - [[Validation|Validation]]
- [[Form-Validation]] - [[Form-Validation|Form validation]]
- [[Handle-Form]] - [[Handle-Form|Form handling]]
