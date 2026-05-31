# How to Use Callbacks

## Basic Callback

```javascript
function greet(name, callback) {
    console.log(`Hello, ${name}!`);
    callback();
}

greet("John", function() {
    console.log("Welcome!");
});
```

## Callbacks in Array Methods

```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach
numbers.forEach(function(num) {
    console.log(num);
});

// map
const doubled = numbers.map(function(num) {
    return num * 2;
});

// filter
const evens = numbers.filter(function(num) {
    return num % 2 === 0;
});
```

## Callback Hell

```javascript
// Nested callbacks (hard to read)
getUser(userId, function(user) {
    getOrders(user.id, function(orders) {
        getOrderDetails(orders[0].id, function(details) {
            console.log(details);
        });
    });
});
```

## Callback vs Promise vs Async/Await

```javascript
// Callback
getData(function(result) {
    console.log(result);
});

// Promise
getData()
    .then(result => console.log(result));

// Async/Await
const result = await getData();
console.log(result);
```

## Quick Revision

- Callback = function passed as argument
- Used for: array methods, async operations, events
- Can lead to "callback hell" (deep nesting)
- Prefer [[What-is-Promise|Promises]]/[[What-is-AsyncAwait|async-await]] for async code
- Still used in sync operations

---

## Related Topics

- [[What-is-Callback]] - [[What-is-Callback|Callbacks]] overview
- [[What-is-Function]] - [[What-is-Function|Functions]]
- [[What-is-Promise]] - [[What-is-Promise|Promises]]
- [[What-is-AsyncAwait]] - [[What-is-AsyncAwait|Async/await]]
- [[Write-Callback]] - [[Write-Callback|Writing callbacks]]
