# What is an [[IIFE]]?

## Definition

An [[IIFE]] (Immediately Invoked Function Expression) is a [[Function]] that **runs immediately** after it's defined.

## Syntax

```javascript
// Basic IIFE
(function() {
    console.log("Runs immediately!");
})();

// With parameters
(function(name) {
    console.log(`Hello, ${name}!`);
})("John");

// Arrow function IIFE
(() => {
    console.log("Arrow IIFE!");
})();
```

## Why Use IIFEs

```javascript
// 1. Avoid polluting global scope
(function() {
    const private = "I'm private";
    console.log(private);
})();

// 2. Initialize modules
const module = (function() {
    let count = 0;
    return {
        increment: () => ++count,
        getCount: () => count
    };
})();

// 3. Async operations
(async function() {
    const data = await fetch("https://api.example.com");
    console.log(data);
})();
```

## Quick Revision

- [[IIFE]] = [[Function]] that runs immediately
- Syntax: `(function() {})()`
- Avoids global [[Scope]] pollution
- Used for initialization and encapsulation
- Can be async with `[[Async]]`

---

## Related Topics

- [[What-is-Function]] - Functions
- [[Write-IIFE]] - Writing IIFEs
- [[What-is-Scope]] - Scope
- [[What-is-Closure]] - Closures
- [[What-is-Module]] - Modules