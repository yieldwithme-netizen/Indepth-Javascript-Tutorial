# How to Write Function Expressions

## Basic Syntax

```javascript
// Named function expression
const greet = function greet(name) {
    return `Hello, ${name}!`;
};

// Anonymous function expression
const greet = function(name) {
    return `Hello, ${name}!`;
};
```

## Function Declaration vs Expression

```javascript
// Declaration (hoisted)
greet(); // Works
function greet() {
    console.log("Hello");
}

// Expression (NOT hoisted)
sayHi(); // Error!
const sayHi = function() {
    console.log("Hi");
};
```

## IIFE (Immediately Invoked)

```javascript
// Runs immediately
(function() {
    console.log("Runs immediately!");
})();

// With parameters
(function(name) {
    console.log(`Hello, ${name}!`);
})("John");
```

## Common Use Cases

```javascript
// Callbacks
setTimeout(function() {
    console.log("After 1 second");
}, 1000);

// Array methods
const doubled = [1, 2, 3].map(function(num) {
    return num * 2;
});

// Module pattern
const module = (function() {
    let private = "I'm private";
    return {
        public: function() { return private; }
    };
})();
```

## Quick Revision

- Function expression = function assigned to variable
- NOT hoisted (must declare before use)
- Can be named or anonymous
- [[What-is-IIFE|IIFE]] = runs immediately
- Often used for callbacks

---

## Related Topics

- [[What-is-Expression]] - [[What-is-Expression|Function expressions]] overview
- [[What-is-Function]] - [[What-is-Function|Functions]]
- [[What-is-IIFE]] - [[What-is-IIFE|IIFE]]
- [[Declare-Function]] - [[Declare-Function|Function declarations]]
- [[What-is-Callback]] - [[What-is-Callback|Callbacks]]
