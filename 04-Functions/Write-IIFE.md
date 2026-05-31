# How to Write IIFE

## Basic Syntax

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

## Common Patterns

```javascript
// jQuery style
(function($) {
    // $ is jQuery here
})(jQuery);

// Module pattern
const counter = (function() {
    let count = 0;
    return {
        increment: function() { count++; },
        getCount: function() { return count; }
    };
})();

// Async IIFE
(async () => {
    const response = await fetch("/api/data");
    const data = await response.json();
    console.log(data);
})();
```

## Quick Revision

- IIFE = function that runs immediately
- Syntax: `(function() {})()`
- Avoids global scope pollution
- Used for initialization and encapsulation
- Can be [[What-is-AsyncAwait|async]] with `async`

---

## Related Topics

- [[What-is-IIFE]] - [[What-is-IIFE|IIFE]] overview
- [[What-is-Function]] - [[What-is-Function|Functions]]
- [[Write-IIFE]] - [[Write-IIFE|Writing IIFEs]]
- [[What-is-Scope]] - [[What-is-Scope|Scope]]
- [[What-is-Closure]] - [[What-is-Closure|Closures]]
- [[What-is-Module]] - [[What-is-Module|Modules]]
