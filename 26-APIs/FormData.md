# FormData

## Definition

FormData **collects form data** for submission.

## Example

```javascript
const form = document.querySelector('form');
const formData = new FormData(form);

// Add data
formData.append('key', 'value');

// Send
fetch('/upload', {
    method: 'POST',
    body: formData
});
```

## Quick Revision

- FormData collects form fields
- `append()` to add data
- Send as fetch body
- Automatically sets Content-Type

---

## Related Topics

- [[What-is-FormEvent]] - [[What-is-FormEvent|Form events]]
- [[Handle-Form]] - [[Handle-Form|Form handling]]
- [[FormData]] - [[FormData|FormData]]
- [[What-is-Fetch]] - [[What-is-Fetch|Fetch]]
