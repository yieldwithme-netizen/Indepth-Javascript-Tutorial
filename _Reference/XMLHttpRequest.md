# XMLHttpRequest

## Definition

XMLHttpRequest (XHR) makes **HTTP requests** in the browser.

## Basic Usage

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
- `send()` to execute
- Use Fetch API instead

---

## Related Topics

- [[What-is-Fetch]] - [[What-is-Fetch|Fetch]]
- [[XMLHttpRequest]] - [[XMLHttpRequest|XHR]]
- [[Make-HTTP]] - [[Make-HTTP|HTTP requests]]
- [[HTTP-Requests]] - [[HTTP-Requests|HTTP requests]]
