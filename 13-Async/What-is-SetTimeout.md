# What is setTimeout

`setTimeout` is a browser/Node.js API that executes a function **once** after a specified delay in milliseconds.

## Definition

```javascript
const timeoutId = setTimeout(callback, delay);
```

- `callback` — Function to execute after the delay
- `delay` — Time in milliseconds (1000ms = 1 second)
- Returns a `timeoutId` for cancellation

## Basic Usage

```javascript
console.log("Start");

setTimeout(() => {
  console.log("After 2 seconds");
}, 2000);

console.log("End");

// Output:
// Start
// End
// After 2 seconds
```

## Passing Arguments

```javascript
function greet(name, greeting) {
  console.log(`${greeting}, ${name}!`);
}

setTimeout(greet, 1000, "Alice", "Hello");
// "Hello, Alice!" after 1 second
```

## Clearing Timeout

```javascript
const id = setTimeout(() => {
  console.log("This will never run");
}, 5000);

clearTimeout(id); // Cancelled
```

## Common Use Cases

### Delayed Execution
```javascript
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function run() {
  console.log("Waiting...");
  await delay(2000);
  console.log("Done!");
}
```

### Debouncing Input
```javascript
let timeoutId;
input.addEventListener("input", (e) => {
  clearTimeout(timeoutId);
  timeoutId = setTimeout(() => {
    console.log("Search:", e.target.value);
  }, 300);
});
```

### Retry Logic
```javascript
function retry(fn, delay, maxAttempts) {
  return new Promise((resolve, reject) => {
    let attempts = 0;

    function attempt() {
      attempts++;
      fn()
        .then(resolve)
        .catch(err => {
          if (attempts < maxAttempts) {
            setTimeout(attempt, delay);
          } else {
            reject(err);
          }
        });
    }

    attempt();
  });
}
```

### Auto-hide Notifications
```javascript
function showNotification(message, duration = 3000) {
  const el = document.createElement("div");
  el.textContent = message;
  document.body.appendChild(el);

  setTimeout(() => {
    el.remove();
  }, duration);
}
```

## How setTimeout Works

```javascript
// JavaScript is single-threaded
// setTimeout does NOT pause execution
console.log("1");

setTimeout(() => console.log("2"), 0); // Even 0ms is async

console.log("3");

// Output: 1, 3, 2
```

## Common Mistakes

- Expecting `setTimeout` to block execution
- Using `setTimeout(fn, 0)` for sync operations
- Forgetting to clear timeouts causing memory leaks
- Relying on exact timing (delays are minimum, not guaranteed)

```javascript
// Wrong - 0ms doesn't mean immediate
setTimeout(() => console.log("async"), 0);
console.log("sync");
// Output: sync, async
```

## Quick Revision

- `setTimeout` executes code once after a delay
- Returns an ID that can be used with `clearTimeout`
- Does NOT block other code from running
- Minimum delay is ~4ms in browsers (even with 0ms)
- Use for debouncing, delays, and one-time delayed actions

## Related Topics

- [[What-is-SetInterval]]
- [[Clear-Timers]]
- [[Promises-and-Async-Await]]
- [[Event-Loop-and-Call-Stack]]
- [[Debounce-and-Throttle]]
