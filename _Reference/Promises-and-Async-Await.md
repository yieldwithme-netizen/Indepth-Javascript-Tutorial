# Promises and Async/Await

## Definition

Promises and async/await handle **asynchronous operations** in JavaScript.

## Promises

```javascript
// Creating
const promise = new Promise((resolve, reject) => {
    const success = true;
    if (success) resolve("Done!");
    else reject("Failed!");
});

// Using
promise
    .then(result => console.log(result))
    .catch(error => console.log(error));
```

## Async/Await

```javascript
// Async function
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

- Promise: `.then()`/`.catch()`
- Async/await: `await` pauses
- Use try/catch with async/await
- Both handle async operations

---

## Related Topics

- [[What-is-Promise]] - [[What-is-Promise|Promises]]
- [[What-is-AsyncAwait]] - [[What-is-AsyncAwait|Async/await]]
- [[Promises-and-Async-Await]] - [[Promises-and-Async-Await|Promises and async/await]]
- [[Promises]] - [[Promises|Promises]]
- [[Async-Await]] - [[Async-Await|Async/await]]
