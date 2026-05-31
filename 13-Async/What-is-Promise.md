# What is a Promise?

## Definition

A Promise is an **object representing the eventual completion or failure** of an async operation.

## States

```javascript
// Promise states:
// 1. Pending: initial state
// 2. Fulfilled: operation completed
// 3. Rejected: operation failed
```

## Creating Promises

```javascript
const promise = new Promise((resolve, reject) => {
    const success = true;
    
    if (success) {
        resolve("Done!"); // fulfilled
    } else {
        reject("Failed!"); // rejected
    }
});
```

## Using Promises

```javascript
promise
    .then(result => console.log(result))  // "Done!"
    .catch(error => console.log(error))   // "Failed!"
    .finally(() => console.log("Always runs"));
```

## Promise Chaining

```javascript
getUser(userId)
    .then(user => getOrders(user.id))
    .then(orders => getOrderDetails(orders[0].id))
    .then(details => console.log(details))
    .catch(error => console.log(error));
```

## Quick Revision

- Promise = async operation result
- States: pending → fulfilled/rejected
- `.then()` for success, `.catch()` for error
- `.finally()` always runs
- Chain with `.then().then()`

---

## Related Topics

- [[What-is-Promise]] - Promises overview
- [[Create-Promise]] - Creating promises
- [[What-is-ThenCatch]] - then/catch
- [[What-is-AsyncAwait]] - Async/await
- [[What-is-Callback]] - Callbacks
