# Callbacks

## Definition
A callback is a function passed as an argument to another function, which is then invoked inside the outer function to complete some kind of routine or action. Callbacks are fundamental to JavaScript's asynchronous behavior.

## Basic Examples

### Synchronous Callback
```javascript
function processArray(arr, callback) {
  const result = [];
  for (const item of arr) {
    result.push(callback(item));
  }
  return result;
}

const numbers = [1, 2, 3, 4, 5];
const doubled = processArray(numbers, (num) => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]
```

### Asynchronous Callback
```javascript
function fetchData(url, callback) {
  setTimeout(() => {
    const data = { id: 1, name: 'John' };
    callback(data);
  }, 1000);
}

fetchData('/api/user', (data) => {
  console.log(data); // { id: 1, name: 'John' }
});
```

## Real-World Examples

### Event Handlers
```javascript
document.getElementById('button').addEventListener('click', function() {
  console.log('Button clicked!');
});
```

### Array Methods
```javascript
const fruits = ['apple', 'banana', 'cherry'];

// These all use callbacks
fruits.forEach((fruit) => console.log(fruit));
const upper = fruits.map((fruit) => fruit.toUpperCase());
const filtered = fruits.filter((fruit) => fruit.startsWith('b'));
```

### Node.js Style Callback (Error-First)
```javascript
function readFileSystem(path, callback) {
  // Simulating file read
  setTimeout(() => {
    if (path) {
      callback(null, 'File content');
    } else {
      callback(new Error('Path required'));
    }
  }, 100);
}

readFileSystem('/path/to/file', (error, data) => {
  if (error) {
    console.error(error);
    return;
  }
  console.log(data); // 'File content'
});
```

### Callback Hell (Pyramid of Doom)
```javascript
// Avoid this pattern!
getUser(userId, (user) => {
  getOrders(user.id, (orders) => {
    getOrderDetails(orders[0].id, (details) => {
      getShipping(details.shippingId, (shipping) => {
        console.log(shipping);
      });
    });
  });
});
```

## Common Use Cases
- Event handling (clicks, scrolls, etc.)
- Asynchronous operations (API calls, file I/O)
- Array iteration methods
- Timers (setTimeout, setInterval)
- Node.js operations

## Common Mistakes
- Callback hell (deeply nested callbacks)
- Not handling errors in callbacks
- Forgetting to return from callback functions
- Mixing up callback parameters
- Not checking if callback is a function

## Quick Revision Summary
- Callbacks are functions passed as arguments
- Used for synchronous and asynchronous operations
- Essential for event handling
- Can lead to "callback hell" when deeply nested
- Prefer Promises or async/await for complex async flows

## Related Topics
- [[Promises]]
- [[Async-Await]]
- [[Higher-Order-Functions]]
- [[Events]]
- [[DOM]]
