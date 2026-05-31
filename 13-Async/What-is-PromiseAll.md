# What is Promise.all()

## Definition

`Promise.all()` takes an **iterable of promises** and returns a single promise that resolves when all input promises have resolved, or rejects if any one of them rejects. It is used for **parallel execution** of independent async operations.

## Syntax

```javascript
const promise = Promise.all([promise1, promise2, promise3]);
```

## Code Example: Basic Usage

```javascript
const p1 = new Promise((resolve) => setTimeout(() => resolve("one"), 1000));
const p2 = new Promise((resolve) => setTimeout(() => resolve("two"), 500));
const p3 = new Promise((resolve) => setTimeout(() => resolve("three"), 1500));

Promise.all([p1, p2, p3]).then((results) => {
  console.log(results); // ["one", "two", "three"]
});

// Total time: ~1.5s (not 3s) because they run in parallel
```

## Code Example: Real-World — Parallel API Calls

```javascript
async function loadDashboard() {
  try {
    const [user, posts, notifications] = await Promise.all([
      fetch("/api/user").then((r) => r.json()),
      fetch("/api/posts").then((r) => r.json()),
      fetch("/api/notifications").then((r) => r.json()),
    ]);

    console.log("User:", user);
    console.log("Posts:", posts);
    console.log("Notifications:", notifications);
  } catch (error) {
    console.error("One of the requests failed:", error);
  }
}
```

## Code Example: Map to Promises

```javascript
const userIds = [1, 2, 3, 4, 5];

const userPromises = userIds.map((id) =>
  fetch(`/api/users/${id}`).then((r) => r.json())
);

Promise.all(userPromises)
  .then((users) => {
    console.log("All users:", users);
  })
  .catch((err) => {
    console.error("Failed to fetch a user:", err);
  });
```

## Rejection Behavior

`Promise.all()` **rejects immediately** if any promise rejects. The remaining promises continue executing but their results are ignored.

```javascript
const fast = new Promise((resolve) => setTimeout(() => resolve("fast"), 500));
const fails = new Promise((_, reject) => setTimeout(() => reject("error"), 1000));
const slow = new Promise((resolve) => setTimeout(() => resolve("slow"), 2000));

Promise.all([fast, fails, slow])
  .then((results) => console.log(results))
  .catch((err) => console.error(err)); // "error" (at 1s, not 2s)

// Note: `slow` still resolves at 2s — Promise.all just ignores it
```

## Promise.allSettled() — Alternative

If you want **all results regardless of failure**, use `Promise.allSettled()`:

```javascript
const promises = [
  Promise.resolve("success"),
  Promise.reject("failure"),
  Promise.resolve("another success"),
];

Promise.allSettled(promises).then((results) => {
  results.forEach((result) => {
    if (result.status === "fulfilled") {
      console.log("Resolved:", result.value);
    } else {
      console.log("Rejected:", result.reason);
    }
  });
});

// Output:
// Resolved: success
// Rejected: failure
// Resolved: another success
```

| Method | Resolves when | Rejects when |
|--------|---------------|--------------|
| `Promise.all()` | All promises resolve | Any promise rejects |
| `Promise.allSettled()` | All promises settle (resolve or reject) | Never |
| `Promise.race()` | First promise settles | First promise rejects |

## Common Use Cases

- **Parallel API calls** — fetch multiple resources simultaneously
- **Loading multiple files** — read several files concurrently
- **Form validation** — check multiple async validations at once
- **Batch operations** — process multiple items in parallel

## Common Mistakes

1. **Not handling rejection** — one failure breaks everything
```javascript
// Bad: no error handling
Promise.all([fetch("/api/a"), fetch("/api/b")])
  .then(([a, b]) => console.log(a, b));

// Good
Promise.all([fetch("/api/a"), fetch("/api/b")])
  .then(([a, b]) => console.log(a, b))
  .catch((err) => console.error(err));
```

2. **Using it when operations depend on each other** — use sequential `await` instead
```javascript
// Bad: getPosts depends on userId
const [user, posts] = await Promise.all([getUser(), getPosts()]); // getPosts needs userId!

// Good
const user = await getUser();
const posts = await getPosts(user.id);
```

3. **Forgetting that non-promise values are wrapped**
```javascript
Promise.all([1, 2, 3]).then((results) => {
  console.log(results); // [1, 2, 3] — values are auto-wrapped
});
```

## Quick Revision Summary

- `Promise.all()` runs multiple promises in parallel
- Resolves when ALL promises resolve, rejects if ANY rejects
- Returns results in the same order as input
- Use `Promise.allSettled()` if you need all results regardless of failure
- Perfect for independent async operations that can run concurrently

## Related Topics

- [[Create-Promise]]
- [[What-is-ThenCatch]]
- [[Use-AsyncAwait]]
- [[What-is-PromiseRace]]
