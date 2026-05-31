# What is Callback Hell

## Definition

Callback hell (also called the **"Pyramid of Doom"**) is an anti-pattern that occurs when multiple asynchronous operations are nested deeply inside callbacks. This leads to code that is hard to read, debug, and maintain.

## What It Looks Like

```javascript
getData(function (a) {
  getMoreData(a, function (b) {
    getEvenMoreData(b, function (c) {
      getYetMoreData(c, function (d) {
        console.log(d);
      });
    });
  });
});
```

Each callback depends on the result of the previous one, creating a deeply indented triangle of code.

## A Real-World Example

```javascript
// Simulating sequential async operations (e.g., login → fetch profile → fetch posts)
function login(username, password, callback) {
  setTimeout(() => {
    callback({ token: "abc123" });
  }, 1000);
}

function getProfile(token, callback) {
  setTimeout(() => {
    callback({ name: "Alice", age: 30 });
  }, 1000);
}

function getPosts(token, callback) {
  setTimeout(() => {
    callback([{ id: 1, title: "Hello" }, { id: 2, title: "World" }]);
  }, 1000);
}

// Callback Hell version
login("alice", "pass123", function (result) {
  console.log("Logged in");
  getProfile(result.token, function (profile) {
    console.log("Profile:", profile);
    getPosts(result.token, function (posts) {
      console.log("Posts:", posts);
    });
  });
});
```

## Problems with Callback Hell

| Problem | Description |
|---------|-------------|
| **Readability** | Deep indentation makes code hard to follow |
| **Error handling** | Each callback needs its own error handling |
| **Debugging** | Stack traces become misleading |
| **Maintenance** | Adding new steps means re-indenting everything |
| **Logic flow** | Sequential steps are buried in nesting |

## Error Handling Nightmare

```javascript
getData(function (err, a) {
  if (err) {
    console.error(err);
    return;
  }
  getMoreData(a, function (err, b) {
    if (err) {
      console.error(err);
      return;
    }
    getEvenMoreData(b, function (err, c) {
      if (err) {
        console.error(err);
        return;
      }
      console.log(c);
    });
  });
});
```

## How to Fix It

### 1. Named Functions (Flattening)

```javascript
function handleC(result) {
  console.log("C:", result);
}

function handleB(result) {
  getEvenMoreData(result, handleC);
}

function handleA(result) {
  getMoreData(result, handleB);
}

getData(handleA);
```

### 2. Promises (Recommended)

```javascript
getData()
  .then((a) => getMoreData(a))
  .then((b) => getEvenMoreData(b))
  .then((c) => console.log(c))
  .catch((err) => console.error(err));
```

### 3. Async/Await (Best)

```javascript
async function fetchData() {
  try {
    const a = await getData();
    const b = await getMoreData(a);
    const c = await getEvenMoreData(b);
    console.log(c);
  } catch (err) {
    console.error(err);
  }
}
```

## Common Mistakes

1. **Nesting when you don't need to** — independent callbacks should not be nested
2. **Ignoring errors at any level** — one unhandled error breaks the chain
3. **Not using promises when available** — most modern APIs return promises

```javascript
// Unnecessarily nested (independent operations)
getUser(function (user) {
  getSettings(function (settings) {
    // These are independent! Run them in parallel
  });
});

// Better
Promise.all([getUser(), getSettings()]).then(([user, settings]) => {
  // runs in parallel
});
```

## Quick Revision Summary

- Callback hell is deeply nested async callbacks forming a pyramid
- It makes code hard to read, debug, and maintain
- Fix it with named functions, promises, or async/await
- Always handle errors at every level if using raw callbacks
- Prefer `Promise.all` for independent async operations

## Related Topics

- [[What-is-Sync]]
- [[What-is-Async]]
- [[Create-Promise]]
- [[What-is-ThenCatch]]
- [[Use-AsyncAwait]]
- [[What-is-PromiseAll]]
