# Network Requests

## Definition

Network requests **fetch data from servers**.

## Fetch API

```javascript
fetch("https://api.example.com/data")
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

## Async/Await

```javascript
async function getData() {
    const response = await fetch("https://api.example.com/data");
    const data = await response.json();
    return data;
}
```

## Quick Revision

- Fetch API for HTTP requests
- `response.json()` to parse JSON
- Use `async/await` for cleaner code
- Handle errors with `try/catch`

---

## Related Topics

- [[What-is-Fetch]] - [[What-is-Fetch|Fetch]]
- [[Make-HTTP]] - [[Make-HTTP|HTTP requests]]
- [[Network-Requests]] - [[Network-Requests|Network requests]]
- [[HTTP-Requests]] - [[HTTP-Requests|HTTP requests]]
