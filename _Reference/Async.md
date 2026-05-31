# Async JavaScript

Asynchronous programming allows JavaScript to perform long-running tasks without blocking the main thread, enabling responsive applications.

## Definition

Async JavaScript is a programming paradigm that handles operations that take time (network requests, file I/O, timers) by executing them in the background while the main thread continues running.

## Callbacks

```javascript
// Traditional callback pattern
function fetchData(callback) {
    setTimeout(() => {
        const data = { name: 'John', age: 30 };
        callback(null, data);
    }, 1000);
}

fetchData((error, data) => {
    if (error) {
        console.error(error);
        return;
    }
    console.log(data);
});

// Callback hell example
getUser(userId, (err, user) => {
    getOrders(user.id, (err, orders) => {
        getOrderDetails(orders[0].id, (err, details) => {
            console.log(details);
        });
    });
});
```

## Promises

```javascript
// Creating a Promise
function fetchData() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const data = { name: 'John', age: 30 };
            resolve(data);
        }, 1000);
    });
}

// Using Promises
fetchData()
    .then(data => console.log(data))
    .catch(error => console.error(error))
    .finally(() => console.log('Done'));

// Promise chaining
getUser(userId)
    .then(user => getOrders(user.id))
    .then(orders => getOrderDetails(orders[0].id))
    .then(details => console.log(details))
    .catch(error => console.error(error));

// Promise.all - parallel execution
const promise1 = fetch('/api/users');
const promise2 = fetch('/api/posts');
const promise3 = fetch('/api/comments');

Promise.all([promise1, promise2, promise3])
    .then(([users, posts, comments]) => {
        console.log({ users, posts, comments });
    });

// Promise.allSettled - wait for all regardless of outcome
Promise.allSettled([promise1, promise2, promise3])
    .then(results => {
        results.forEach(result => {
            if (result.status === 'fulfilled') {
                console.log('Success:', result.value);
            } else {
                console.log('Failed:', result.reason);
            }
        });
    });

// Promise.race - first to resolve/reject wins
Promise.race([promise1, promise2])
    .then(fastest => console.log(fastest));

// Promise.any - first to resolve wins (ignores rejections)
Promise.any([promise1, promise2])
    .then(first => console.log(first));
```

## Async/Await

```javascript
// Async function always returns a Promise
async function fetchData() {
    const response = await fetch('/api/data');
    const data = await response.json();
    return data;
}

// Using async/await
async function main() {
    try {
        const data = await fetchData();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}

// Parallel execution with async/await
async function loadDashboard() {
    const [users, posts, comments] = await Promise.all([
        fetch('/api/users').then(r => r.json()),
        fetch('/api/posts').then(r => r.json()),
        fetch('/api/comments').then(r => r.json())
    ]);
    
    return { users, posts, comments };
}

// Sequential execution
async function sequential() {
    const result1 = await step1();
    const result2 = await step2(result1);
    const result3 = await step3(result2);
    return result3;
}
```

## Common Use Cases

- Fetching data from APIs
- Reading/writing files
- Timers and delays
- Web animations
- Database operations
- WebSocket communication

## Common Mistakes

1. **Blocking the main thread** - Using synchronous operations in async context
2. **Not handling errors** - Always use try/catch with async/await
3. **Unnecessary sequential awaits** - Use Promise.all for independent operations
4. **Forgetting async functions return Promises** - Always return values properly
5. **Creating unhandled Promise rejections** - Always add catch handlers

## Related Topics

- [[Promises]]
- [[Async/Await]]
- [[Error Handling]]
- [[Event Loop]]
- [[Callbacks]]

## Quick Revision

| Approach | Syntax | Error Handling |
|----------|--------|----------------|
| Callback | `fn(err, data)` | Error parameter |
| Promise | `.then().catch()` | `.catch()` |
| Async/Await | `await promise` | `try/catch` |
| Parallel | `Promise.all([...])` | `.catch()` or `try/catch` |
