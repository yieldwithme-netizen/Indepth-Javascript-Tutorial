# What is Asynchronous Code

## Definition

Asynchronous code allows JavaScript to perform long-running tasks (like network requests, timers, or file I/O) **without blocking** the main thread. Instead of waiting for a task to finish, JavaScript fires off the task and continues executing the rest of the code. When the task completes, a callback or promise resolves with the result.

## Why Async Exists

JavaScript is **single-threaded** — it can only do one thing at a time. If network requests were synchronous, the entire UI would freeze every time data was fetched. Async solves this by offloading work to the browser/Node.js runtime (via Web APIs, libuv, etc.).

```
Synchronous:
[Fetch data (3s)] → [Rest of code waits 3s] → [Rest of code runs]

Asynchronous:
[Start fetch (3s)] → [Rest of code runs immediately] → [Fetch completes later]
```

## Code Example: setTimeout

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Inside timeout");
}, 2000);

console.log("End");

// Output:
// Start
// End
// Inside timeout (after 2 seconds)
```

The timeout does not block — `console.log("End")` runs immediately.

## Code Example: Callback Pattern

```javascript
function fetchData(callback) {
  setTimeout(() => {
    const data = { id: 1, name: "Alice" };
    callback(data);
  }, 1000);
}

console.log("Before fetch");
fetchData((data) => {
  console.log("Data received:", data);
});
console.log("After fetch");

// Output:
// Before fetch
// After fetch
// Data received: { id: 1, name: "Alice" }
```

## Code Example: Promises

```javascript
function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({ id: 1, name: "Alice" });
    }, 1000);
  });
}

console.log("Before");
fetchData().then((data) => console.log(data));
console.log("After");

// Output:
// Before
// After
// { id: 1, name: "Alice" }
```

## Code Example: Async/Await

```javascript
async function getData() {
  console.log("Before");
  const data = await fetchData(); // pauses here, doesn't block
  console.log("Data:", data);
  console.log("After");
}

getData();

// Output:
// Before
// (1 second pause)
// Data: { id: 1, name: "Alice" }
// After
```

## Common Async Operations

| Operation | API | Returns |
|-----------|-----|---------|
| Timers | `setTimeout`, `setInterval` | void (uses callback) |
| Network requests | `fetch` | Promise |
| File I/O (Node) | `fs.readFile` | callback / Promise |
| Event listeners | `addEventListener` | void (uses callback) |

## Common Use Cases

- **Fetching data** from APIs
- **Reading/writing files** on disk
- **Timers** and scheduled tasks
- **Database queries**
- **User interactions** — click events, keyboard input

## Common Mistakes

1. **Expecting async code to run immediately**
```javascript
let result;
fetch("https://api.example.com/data")
  .then((res) => res.json())
  .then((data) => { result = data; });
console.log(result); // undefined — not yet assigned
```

2. **Not handling errors**
```javascript
// Bad: error silently swallowed
fetch("https://bad-url.example.com");

// Good: always handle errors
fetch("https://bad-url.example.com")
  .catch((err) => console.error(err));
```

3. **Forgetting that async code is non-blocking**
```javascript
console.log("1");
setTimeout(() => console.log("2"), 0);
console.log("3");
// Output: 1, 3, 2 — NOT 1, 2, 3
```

## Quick Revision Summary

- Async code runs without blocking the main thread
- JavaScript achieves this via callbacks, promises, and async/await
- Async operations complete later — code after them runs immediately
- Always handle errors in async code
- Async is essential for UI responsiveness and I/O operations

## Related Topics

- [[What-is-Sync]]
- [[What-is-CallbackHell]]
- [[Create-Promise]]
- [[What-is-ThenCatch]]
- [[Use-AsyncAwait]]
- [[What-is-Fetch]]
