# HTTP Requests

## Definition

HTTP requests **fetch data from servers** using the Fetch API or XMLHttpRequest.

## Fetch API

```javascript
// GET request
fetch("https://api.example.com/users")
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));

// POST request
fetch("https://api.example.com/users", {
    method: "POST",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify({ name: "John" })
})
    .then(response => response.json())
    .then(data => console.log(data));
```

## Async/Await

```javascript
async function getUsers() {
    try {
        const response = await fetch("https://api.example.com/users");
        const data = await response.json();
        return data;
    } catch (error) {
        console.error("Error:", error);
    }
}
```

## Quick Revision

- Fetch API for HTTP requests
- `fetch()` returns a Promise
- Use `response.json()` to parse JSON
- Use `async/await` for cleaner code
- Handle errors with `try/catch`

---

## Related Topics

- [[What-is-Fetch]] - [[What-is-Fetch|Fetch API]]
- [[Make-HTTP]] - [[Make-HTTP|HTTP requests]]
- [[What-is-Promise]] - [[What-is-Promise|Promises]]
- [[What-is-AsyncAwait]] - [[What-is-AsyncAwait|Async/await]]
