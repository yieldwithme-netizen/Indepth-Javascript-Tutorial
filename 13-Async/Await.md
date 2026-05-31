# Await

## Definition

`await` pauses an **async function** until a Promise resolves.

## Basic Usage

```javascript
async function fetchData() {
    const response = await fetch("https://api.example.com");
    const data = await response.json();
    return data;
}
```

## Error Handling

```javascript
async function fetchData() {
    try {
        const response = await fetch("https://api.example.com");
        const data = await response.json();
        return data;
    } catch (error) {
        console.error("Error:", error);
    }
}
```

## Quick Revision

- `await` pauses until Promise resolves
- Only works in `async` functions
- Use `try/catch` for errors
- Makes async code look synchronous

---

## Related Topics

- [[What-is-AsyncAwait]] - [[What-is-AsyncAwait|Async/await]]
- [[Use-AsyncAwait]] - [[Use-AsyncAwait|Using async/await]]
- [[What-is-Promise]] - [[What-is-Promise|Promises]]
