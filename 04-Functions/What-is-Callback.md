# What is a [[Callback]] Function?

## Definition

A [[Callback]] is a **[[Function]] passed as an [[Argument|argument]]** to another [[Function]], to be called later.

## Basic Example

```javascript
function greet(name, callback) {
    console.log(`Hello, ${name}!`);
    callback();
}

greet("John", function() {
    console.log("Welcome!");
});
```

## [[Callback]]s in [[Array]] Methods

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

## [[Callback]] Hell

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

## [[Callback]] vs [[Promise]] vs [[Async]]/[[Await]]

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

- [[Callback]] = [[Function]] passed as [[Argument|argument]]
- Used for: [[Array]] methods, async operations, events
- Can lead to "callback hell" (deep nesting)
- Prefer [[Promise]]s/async-await for async code
- Still used in sync operations

---

## Related Topics

- [[What-is-Function]] - Functions
- [[Use-Callback]] - Using callbacks
- [[What-is-Promise]] - Promises
- [[What-is-AsyncAwait]] - Async/await
- [[Write-Callback]] - Writing callbacks