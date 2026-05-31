# Promise.all

## Definition

`Promise.all` waits for **all promises** to resolve.

## Basic Usage

```javascript
const promise1 = fetch('/api/user');
const promise2 = fetch('/api/posts');

const [user, posts] = await Promise.all([promise1, promise2]);
```

## Error Handling

```javascript
try {
    const results = await Promise.all([promise1, promise2]);
} catch (error) {
    // If any promise rejects
    console.error(error);
}
```

## Quick Revision

- `Promise.all()` waits for all
- Returns array of results
- Fails if any promise rejects
- Use for: parallel operations

---

## Related Topics

- [[What-is-PromiseAll]] - [[What-is-PromiseAll|Promise.all]]
- [[Promise.all]] - [[Promise.all|Promise.all]]
- [[What-is-Promise]] - [[What-is-Promise|Promises]]
- [[What-is-PromiseRace]] - [[What-is-PromiseRace|Promise.race]]
