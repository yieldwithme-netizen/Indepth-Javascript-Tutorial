# What is .then() and .catch()

## Definition

`.then()` and `.catch()` are **methods on Promises** that let you handle the result of an asynchronous operation. `.then()` handles the resolved value, while `.catch()` handles any errors (rejections).

## Syntax

```javascript
promise
  .then((resolvedValue) => {
    // handle success
  })
  .catch((error) => {
    // handle failure
  });
```

## Code Example: Basic Usage

```javascript
function fetchUser() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ id: 1, name: "Alice" });
    }, 1000);
  });
}

fetchUser()
  .then((user) => {
    console.log("User:", user.name);
  })
  .catch((error) => {
    console.error("Error:", error.message);
  });
```

## Code Example: Chaining .then()

Each `.then()` returns a **new promise**, allowing you to chain multiple handlers:

```javascript
function double(x) {
  return new Promise((resolve) => {
    setTimeout(() => resolve(x * 2), 500);
  });
}

double(2)
  .then((result) => {
    console.log(result);      // 4
    return double(result);
  })
  .then((result) => {
    console.log(result);      // 8
    return double(result);
  })
  .then((result) => {
    console.log(result);      // 16
  });
```

## Code Example: Multiple .catch() vs One

```javascript
// Good: single catch at the end handles all errors
fetchUser()
  .then((user) => getPosts(user.id))
  .then((posts) => renderPosts(posts))
  .catch((err) => console.error("Something failed:", err));

// Bad: error in first .then won't be caught by later .catch
fetchUser()
  .then((user) => {
    throw new Error("Oops");
  })
  .catch((err) => console.error(err)); // This WILL catch it
```

## Code Example: Inline vs Named Functions

```javascript
// Inline (good for simple cases)
fetchUser()
  .then((user) => console.log(user.name))
  .catch((err) => console.error(err));

// Named functions (good for complex logic)
function handleUser(user) {
  console.log("Name:", user.name);
  console.log("Email:", user.email);
  return getPosts(user.id);
}

function handlePosts(posts) {
  console.log("Posts count:", posts.length);
}

function handleError(error) {
  console.error("Failed:", error.message);
}

fetchUser()
  .then(handleUser)
  .then(handlePosts)
  .catch(handleError);
```

## .finally()

`.finally()` runs regardless of whether the promise was resolved or rejected:

```javascript
fetchUser()
  .then((user) => console.log(user))
  .catch((err) => console.error(err))
  .finally(() => {
    console.log("Done — loading complete");
    hideSpinner(); // cleanup regardless of outcome
  });
```

## Common Patterns

### Error Recovery

```javascript
fetchUser()
  .then((user) => {
    if (!user.active) {
      throw new Error("User inactive");
    }
    return user;
  })
  .catch(() => {
    // recover with a default user
    return { id: 0, name: "Guest" };
  })
  .then((user) => {
    console.log(user.name); // "Guest" if error occurred
  });
```

### Parallel with Promise.all

```javascript
Promise.all([fetchUser(), fetchPosts(), fetchComments()])
  .then(([user, posts, comments]) => {
    console.log(user, posts, comments);
  })
  .catch((err) => console.error(err));
```

## Common Mistakes

1. **Forgetting `.catch()`** — unhandled rejections crash Node.js or log warnings in browsers
```javascript
// Bad: no error handling
fetch("https://bad-url.example.com")
  .then((res) => res.json());
```

2. **Returning inside `.then()` without `return`**
```javascript
// Bad: next .then receives undefined
promise.then((val) => {
  doSomethingAsync(val); // no return
}).then((result) => {
  console.log(result); // undefined
});

// Good
promise.then((val) => {
  return doSomethingAsync(val);
}).then((result) => {
  console.log(result);
});
```

3. **Mixing async/await with unnecessary `.then()` chains** — pick one style and stay consistent.

## Quick Revision Summary

- `.then()` handles resolved values, `.catch()` handles rejections
- Each `.then()` returns a new promise — enabling chaining
- Always end chains with `.catch()` for error handling
- `.finally()` runs after resolution or rejection for cleanup
- Use named functions for complex chains, inline for simple ones

## Related Topics

- [[Create-Promise]]
- [[Use-AsyncAwait]]
- [[What-is-CallbackHell]]
- [[What-is-PromiseAll]]
