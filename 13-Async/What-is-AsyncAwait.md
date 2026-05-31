# What is async/await?

## Definition

`async/await` is **syntactic sugar** over Promises, making async code look synchronous.

## Basic Syntax

```javascript
// Async function
async function fetchData() {
    const response = await fetch("https://api.example.com");
    const data = await response.json();
    return data;
}

// Arrow function
const fetchData = async () => {
    const response = await fetch("https://api.example.com");
    return await response.json();
};
```

## Error Handling

```javascript
// Try/catch
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

## Parallel Execution

```javascript
// ❌ Sequential (slow)
const users = await fetchUsers();
const posts = await fetchPosts();

// ✅ Parallel (fast)
const [users, posts] = await Promise.all([
    fetchUsers(),
    fetchPosts()
]);
```

## Quick Revision

- `async` before function → returns Promise
- `await` pauses until Promise resolves
- Use `try/catch` for error handling
- `await` only works in async functions
- More readable than `.then()` chains

---

## Related Topics

- [[What-is-AsyncAwait]] - Async/await overview
- [[Use-AsyncAwait]] - Using async/await
- [[What-is-Promise]] - Promises
- [[What-is-Callback]] - Callbacks
- [[Handle-AsyncErrors]] - Error handling
