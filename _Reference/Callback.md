# Callbacks

## Definition

A **callback** is a function passed as an argument to another function, which is then executed ("called back") at a later point. Callbacks are fundamental to JavaScript's asynchronous programming model and are used extensively in event handling, HTTP requests, and array methods.

## Syntax

```javascript
function greet(name, callback) {
  console.log(`Hello, ${name}!`);
  callback();
}

function sayGoodbye() {
  console.log("Goodbye!");
}

greet("Alice", sayGoodbye);
// Output:
// Hello, Alice!
// Goodbye!
```

## Common Use Cases

### 1. Asynchronous Operations

```javascript
function fetchData(url, callback) {
  setTimeout(() => {
    const data = { id: 1, name: "Product" };
    callback(data);
  }, 1000);
}

fetchData("/api/product", (data) => {
  console.log("Received:", data);
});
```

### 2. Event Listeners

```javascript
document.getElementById("btn").addEventListener("click", function () {
  console.log("Button clicked!");
});
```

### 3. Array Methods

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(function (num) {
  return num * 2;
});
console.log(doubled); // [2, 4, 6, 8, 10]

const evens = numbers.filter(function (num) {
  return num % 2 === 0;
});
console.log(evens); // [2, 4]
```

### 4. File System Operations (Node.js)

```javascript
const fs = require("fs");

fs.readFile("data.txt", "utf8", (err, data) => {
  if (err) {
    console.error("Error reading file:", err);
    return;
  }
  console.log("File content:", data);
});
```

## Callback Hell (Pyramid of Doom)

Nested callbacks can lead to hard-to-read code:

```javascript
getUser(userId, (user) => {
  getOrders(user.id, (orders) => {
    getOrderDetails(orders[0].id, (details) => {
      processPayment(details.total, (result) => {
        console.log("Payment done:", result);
      });
    });
  });
});
```

**Solution:** Use Promises or async/await instead.

```javascript
getUser(userId)
  .then((user) => getOrders(user.id))
  .then((orders) => getOrderDetails(orders[0].id))
  .then((details) => processPayment(details.total))
  .then((result) => console.log("Payment done:", result))
  .catch((err) => console.error(err));
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Forgetting to call the callback | Ensure callback is invoked in all code paths |
| Not handling errors in callbacks | Always include error-first pattern in Node.js |
| Mixing up `this` context | Use `.bind()`, arrow functions, or pass context |
| Creating callback hell | Refactor to Promises or async/await |

### Error-First Callbacks (Node.js Convention)

```javascript
function readConfig(path, callback) {
  fs.readFile(path, "utf8", (err, data) => {
    if (err) {
      return callback(err, null); // Error first
    }
    callback(null, JSON.parse(data)); // Success
  });
}

readConfig("config.json", (err, config) => {
  if (err) {
    console.error("Failed to load config:", err);
    return;
  }
  console.log("Config loaded:", config);
});
```

## Quick Revision

- A callback is a function passed as an argument to another function
- Used for asynchronous operations, event handling, and array iteration
- **Error-first callbacks** are the Node.js convention (`err, result`)
- Deeply nested callbacks create **callback hell**
- Prefer **Promises** or **async/await** for complex async flows

## Related Topics

- [[Promises]]
- [[Async-Await]]
- [[Higher-Order-Functions]]
- [[Event-Handling]]
- [[Node-JS]]
