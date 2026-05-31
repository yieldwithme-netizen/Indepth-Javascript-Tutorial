# What-is-XMLHttpRequest

## Definition

XMLHttpRequest makes **HTTP requests** in the browser.

## Example

```javascript
const xhr = new XMLHttpRequest();
xhr.open('GET', 'https://api.example.com/data');
xhr.onload = function() {
    if (xhr.status === 200) {
        const data = JSON.parse(xhr.responseText);
        console.log(data);
    }
};
xhr.send();
```

## Quick Revision

- XHR: older HTTP request method
- `open()` to configure
- `onload` for response
- Use Fetch API instead

---

## Related Topics

- [[What-is-Fetch]] - [[What-is-Fetch|Fetch]]
- [[XMLHttpRequest]] - [[XMLHttpRequest|XHR]]
- [[What-is-XMLHttpRequest]] - [[What-is-XMLHttpRequest|XMLHttpRequest]]
- [[Make-HTTP]] - [[Make-HTTP|HTTP requests]]
