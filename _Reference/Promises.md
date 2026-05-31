# Promises

## Definition

A Promise is an **object representing the eventual completion or failure** of an async operation.

## States

```javascript
// Pending: initial state
// Fulfilled: operation completed
// Rejected: operation failed
```

## Creating Promises

```javascript
const promise = new Promise((resolve, reject) => {
    const success = true;
    
    if (success) {
        resolve("Done!");
    } else {
        reject("Failed!");
    }
});
```

## Using Promises

```javascript
promise
    .then(result => console.log(result))
    .catch(error => console.log(error))
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

- [[What-is-Promise]] - [[What-is-Promise|Promises]] overview
- [[Create-Promise]] - [[Create-Promise|Creating promises]]
- [[What-is-ThenCatch]] - [[What-is-ThenCatch|then/catch]]
- [[What-is-AsyncAwait]] - [[What-is-AsyncAwait|Async/await]]
- [[What-is-Callback]] - [[What-is-Callback|Callbacks]]
