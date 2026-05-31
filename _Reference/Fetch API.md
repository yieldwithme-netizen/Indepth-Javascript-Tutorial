# Fetch API

## Definition

Fetch API makes **HTTP requests** in the browser.

## Basic Usage

```javascript
// GET
fetch("https://api.example.com/data")
    .then(response => response.json())
    .then(data => console.log(data));

// POST
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

## Quick Revision

- Fetch API for HTTP requests
- Returns Promise
- Use `response.json()` to parse JSON
- Use `async/await` for cleaner code

---

## Related Topics

- [[What-is-Fetch]] - [[What-is-Fetch|Fetch]]
- [[Fetch API]] - [[Fetch API|Fetch API]]
- [[Make-HTTP]] - [[Make-HTTP|HTTP requests]]
- [[HTTP-Requests]] - [[HTTP-Requests|HTTP requests]]
