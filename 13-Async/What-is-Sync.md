# What is Synchronous Code

## Definition

Synchronous code is code that executes line by line, in order, one statement at a time. Each line must finish executing before the next line begins. This is the default behavior of JavaScript — it is a **single-threaded, synchronous language**.

## How It Works

JavaScript runs on a **single thread** using a **call stack**. When a function is called, it's pushed onto the stack. When it returns, it's popped off. Nothing else happens until the current function finishes.

```javascript
console.log("First");
console.log("Second");
console.log("Third");

// Output:
// First
// Second
// Third
```

The output is always in order because each `console.log` finishes before the next one starts.

## Code Example: Blocking Behavior

```javascript
function heavyComputation() {
  let sum = 0;
  for (let i = 0; i < 1_000_000_000; i++) {
    sum += i;
  }
  return sum;
}

console.log("Start");
const result = heavyComputation(); // This blocks execution
console.log("End", result);

// Output (after long delay):
// Start
// End 499999999500000000
```

The second `console.log` cannot run until `heavyComputation` finishes completely.

## Synchronous vs Asynchronous

| Feature | Synchronous | Asynchronous |
|---------|-------------|--------------|
| Execution | Line by line | Non-blocking |
| Call Stack | Blocked during execution | Frees up immediately |
| Examples | Math operations, loops, string manipulation | setTimeout, fetch, file I/O |
| UI Impact | Can freeze UI | Keeps UI responsive |

## Common Use Cases

- **Math calculations** — arithmetic, sorting, filtering
- **String operations** — concatenation, splitting, regex matching
- **DOM manipulation** — reading/modifying elements directly
- **Object/array operations** — mapping, reducing, destructuring

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);        // synchronous
const evens = doubled.filter(n => n % 2 === 0); // synchronous
const sum = evens.reduce((a, b) => a + b, 0);   // synchronous

console.log(sum); // 12
```

## Common Mistakes

1. **Blocking the main thread** — long loops or computations freeze the UI
2. **Assuming order guarantees network requests** — network calls are always async
3. **Mixing sync and async expectations** — async code returns before completion

```javascript
// Mistake: Assuming synchronous behavior with setTimeout
console.log("A");
setTimeout(() => console.log("B"), 0);
console.log("C");

// Output: A, C, B (NOT A, B, C)
```

## Quick Revision Summary

- Synchronous code executes line by line, sequentially
- JavaScript is single-threaded and synchronous by default
- Blocking operations freeze the entire program until complete
- Use synchronous code for quick, deterministic tasks
- Avoid long-running synchronous operations in the UI thread

## Related Topics

- [[What-is-Async]]
- [[What-is-CallbackHell]]
- [[Create-Promise]]
- [[Use-AsyncAwait]]
