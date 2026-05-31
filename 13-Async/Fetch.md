# Fetch

## Definition

Fetch makes **HTTP requests** in the browser.

## Example

```javascript
// GET
fetch('https://api.example.com/data')
    .then(res => res.json())
    .then(data => console.log(data));

// POST
fetch('/api/users', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name: 'John' })
});
```

## Quick Revision

- Fetch for HTTP requests
- Returns Promise
- Use response.json() to parse
- Use async/await for cleaner code

---

## Related Topics

- [[What-is-Fetch]] - [[What-is-Fetch|Fetch]]
- [[Fetch]] - [[Fetch|Fetch]]
- [[Make-HTTP]] - [[Make-HTTP|HTTP requests]]
- [[HTTP-Requests]] - [[HTTP-Requests|HTTP requests]]
