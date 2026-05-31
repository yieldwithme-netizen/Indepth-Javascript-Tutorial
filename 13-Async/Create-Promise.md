# How to Create Promises

## Definition

A **Promise** is an object representing the eventual completion or failure of an asynchronous operation. You create a promise using the `Promise` constructor, which takes a function (executor) with two parameters: `resolve` and `reject`.

## Syntax

```javascript
const promise = new Promise((resolve, reject) => {
  // perform async work
  if (success) {
    resolve(value);  // fulfilled
  } else {
    reject(error);   // rejected
  }
});
```

## Code Example: Basic Promise

```javascript
const myPromise = new Promise((resolve, reject) => {
  const success = true;

  if (success) {
    resolve("Operation completed!");
  } else {
    reject("Something went wrong");
  }
});

myPromise
  .then((message) => console.log(message))
  .catch((error) => console.error(error));

// Output: Operation completed!
```

## Code Example: Async Operation with Timer

```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const data = { id: 1, name: "Alice", email: "alice@example.com" };
      resolve(data);
    }, 2000);
  });
}

fetchData().then((data) => {
  console.log("Received:", data);
});
```

## Code Example: Error Handling

```javascript
function divide(a, b) {
  return new Promise((resolve, reject) => {
    if (b === 0) {
      reject(new Error("Cannot divide by zero"));
    } else {
      resolve(a / b);
    }
  });
}

divide(10, 2)
  .then((result) => console.log(result))   // 5
  .catch((err) => console.error(err.message));

divide(10, 0)
  .then((result) => console.log(result))
  .catch((err) => console.error(err.message)); // "Cannot divide by zero"
```

## Promise States

A promise is in one of three states:

```
  ┌──────────────┐
  │   Pending    │  ← initial state
  └──────┬───────┘
         │
    ┌────┴────┐
    ▼         ▼
┌────────┐ ┌────────┐
│Fulfilled│ │Rejected│
└────────┘ └────────┘
  (resolve)  (reject)
```

| State | Meaning |
|-------|---------|
| **Pending** | Operation still in progress |
| **Fulfilled** | Operation completed successfully (`resolve` called) |
| **Rejected** | Operation failed (`reject` called) |

## Code Example: Real-World — API Simulation

```javascript
function getUser(userId) {
  return new Promise((resolve, reject) => {
    const users = {
      1: { id: 1, name: "Alice" },
      2: { id: 2, name: "Bob" },
    };

    setTimeout(() => {
      const user = users[userId];
      if (user) {
        resolve(user);
      } else {
        reject(new Error(`User ${userId} not found`));
      }
    }, 1000);
  });
}

getUser(1).then((user) => console.log(user));
getUser(3).catch((err) => console.error(err.message));
```

## Code Example: Chaining Promises

```javascript
function step1() {
  return new Promise((resolve) => {
    setTimeout(() => resolve(1), 500);
  });
}

function step2(value) {
  return new Promise((resolve) => {
    setTimeout(() => resolve(value + 1), 500);
  });
}

function step3(value) {
  return new Promise((resolve) => {
    setTimeout(() => resolve(value * 10), 500);
  });
}

step1()
  .then(step2)
  .then(step3)
  .then((result) => console.log(result)); // 20
```

## Common Mistakes

1. **Forgetting to return a new promise in `.then`**
```javascript
// Bad: breaks the chain
getData().then((a) => {
  getMoreData(a); // missing return
}).then((b) => {
  console.log(b); // b is undefined
});

// Good
getData().then((a) => {
  return getMoreData(a);
}).then((b) => {
  console.log(b);
});
```

2. **Not returning promises from functions**
```javascript
// Bad
function getData() {
  new Promise((resolve) => {
    resolve("data");
  });
  // returns undefined, not the promise
}

// Good
function getData() {
  return new Promise((resolve) => {
    resolve("data");
  });
}
```

3. **Swallowing errors** — always add `.catch()` at the end of a chain.

## Quick Revision Summary

- Create promises with `new Promise((resolve, reject) => { ... })`
- Call `resolve(value)` on success, `reject(error)` on failure
- A promise has three states: pending, fulfilled, rejected
- Chain promises with `.then()` for sequential async operations
- Always handle rejections with `.catch()`
- Promises solve [[What-is-CallbackHell|callback hell]]

## Related Topics

- [[What-is-Async]]
- [[What-is-CallbackHell]]
- [[What-is-ThenCatch]]
- [[Use-AsyncAwait]]
- [[What-is-PromiseAll]]
