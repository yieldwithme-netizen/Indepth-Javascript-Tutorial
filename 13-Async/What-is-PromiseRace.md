# What is Promise.race()

## Definition

`Promise.race()` takes an **iterable of promises** and returns a single promise that settles (resolves or rejects) as soon as the **first promise** in the iterable settles. The result is the value of that first settled promise.

## Syntax

```javascript
const result = await Promise.race([promise1, promise2, promise3]);
```

## Code Example: Basic Usage

```javascript
const fast = new Promise((resolve) => setTimeout(() => resolve("fast"), 500));
const medium = new Promise((resolve) => setTimeout(() => resolve("medium"), 1000));
const slow = new Promise((resolve) => setTimeout(() => resolve("slow"), 2000));

Promise.race([fast, medium, slow]).then((winner) => {
  console.log(winner); // "fast" (after 500ms)
});

// Only the first result matters — others are ignored
```

## Code Example: Timeout Pattern

The most common use case — adding a timeout to an async operation:

```javascript
function timeout(ms) {
  return new Promise((_, reject) => {
    setTimeout(() => reject(new Error(`Timed out after ${ms}ms`)), ms);
  });
}

async function fetchWithTimeout(url, ms = 5000) {
  return Promise.race([
    fetch(url).then((r) => r.json()),
    timeout(ms),
  ]);
}

fetchWithTimeout("https://api.example.com/data", 3000)
  .then((data) => console.log(data))
  .catch((err) => console.error(err.message)); // "Timed out after 3000ms"
```

## Code Example: Fastest Response

Useful when you have multiple sources for the same data:

```javascript
function fetchFromCDN1() {
  return fetch("https://cdn1.example.com/data").then((r) => r.json());
}

function fetchFromCDN2() {
  return fetch("https://cdn2.example.com/data").then((r) => r.json());
}

Promise.race([fetchFromCDN1(), fetchFromCDN2()])
  .then((data) => {
    console.log("Got data from fastest CDN:", data);
  })
  .catch((err) => {
    console.error("Both CDNs failed:", err);
  });
```

## Rejection Behavior

`Promise.race()` settles as soon as the **first** promise settles (resolve or reject):

```javascript
const resolves = new Promise((resolve) => setTimeout(() => resolve("ok"), 1000));
const rejects = new Promise((_, reject) => setTimeout(() => reject("fail"), 500));

Promise.race([resolves, rejects])
  .then((val) => console.log("Resolved:", val))
  .catch((err) => console.error("Rejected:", err)); // "Rejected: fail" (after 500ms)
```

## Promise.any() — Alternative

`Promise.any()` is similar but only rejects if **all** promises reject:

```javascript
const p1 = Promise.reject("error1");
const p2 = Promise.reject("error2");
const p3 = Promise.resolve("success");

Promise.any([p1, p2, p3])
  .then((result) => console.log(result)) // "success"
  .catch((err) => console.error(err.errors)); // AggregateError if all fail
```

| Method | First resolve | First reject |
|--------|--------------|--------------|
| `Promise.race()` | Returns resolved value | Returns rejected reason |
| `Promise.any()` | Returns resolved value | Ignores, waits for resolve |
| `Promise.all()` | Ignores, waits for all | Returns rejected reason |

## Code Example: User Interaction Race

```javascript
function waitForClick() {
  return new Promise((resolve) => {
    document.getElementById("btn").addEventListener("click", () => {
      resolve("clicked");
    });
  });
}

function waitForTimeout(ms) {
  return new Promise((resolve) => {
    setTimeout(() => resolve("timeout"), ms);
  });
}

Promise.race([waitForClick(), waitForTimeout(5000)]).then((result) => {
  if (result === "clicked") {
    console.log("User clicked the button");
  } else {
    console.log("User didn't click in time");
  }
});
```

## Common Mistakes

1. **Confusing race with all** — race returns only the first settled result
```javascript
// This only gives you ONE result, not all three
Promise.race([fetchA(), fetchB(), fetchC()]);
```

2. **Forgetting that the first rejection wins**
```javascript
// If the first promise rejects, the entire race rejects
const p1 = Promise.reject("error");
const p2 = Promise.resolve("success");

Promise.race([p1, p2])
  .catch((err) => console.log(err)); // "error"
```

3. **Not providing a timeout fallback** — the race may hang if no promise settles
```javascript
// Bad: could hang forever
Promise.race([fetch(url)]);

// Good: always provide a timeout
Promise.race([fetch(url), timeout(5000)]);
```

## Quick Revision Summary

- `Promise.race()` returns the result of the **first settled** promise
- Works for both resolved and rejected promises
- Most useful for **timeout patterns**
- Use `Promise.any()` if you want to ignore the first rejection
- The "losing" promises still execute — they're just ignored

## Related Topics

- [[Create-Promise]]
- [[What-is-PromiseAll]]
- [[Use-AsyncAwait]]
- [[What-is-ThenCatch]]
